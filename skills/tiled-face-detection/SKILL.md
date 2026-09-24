---
name: tiled-face-detection
description: Use when designing or debugging on-device detection of small faces in high-resolution photos using overlapping crops, coordinate restoration, duplicate merging, and bounded image resources.
---

# Tiled face detection

Use this workflow when full-image inference loses small faces because the detector downsamples its input. Tiling makes a face occupy more of an inference input; it does not guarantee detection or anonymity. Keep manual correction available when results are used to suggest privacy masks.

## Prerequisites

- Identify the detector's actual input-size behavior, supported minimum face size, expected orientation, image coordinate origin, and output box format. Measure a fixed set of labeled images before changing the pipeline.
- Define one *upright source-image* coordinate system: normalized `[left, top, right, bottom]` in `[0, 1]`, with origin at top left and positive-area rectangles. Apply EXIF orientation once before preview creation, crops, and any later display mapping.
- Agree on a latency/memory budget, native crop ownership, cancellation policy, and a profile indicating when expensive passes run. Keep source bitmaps/native images on their platforms; share small candidate and tile metadata, not pixel buffers.

## Build and run the passes

1. Preserve the full-image pass so large faces and faces straddling tile boundaries remain detectable. A fast whole-image preview followed by a more accurate upright full-image pass is one working pattern. For a small-face profile, run tile passes *regardless of whether earlier passes found faces*; do not introduce a hidden zero-result fallback. Other profiles may skip tiling explicitly.
2. Define tile bounds once for every platform. **Example profile from blurrf, not a universal requirement:** each axis uses starts `0.0, 0.3, 0.6`, span `0.4`; the Cartesian product yields nine overlapping tiles. Adjacent tiles overlap by `0.1` of the source axis (`25%` of tile width). A face occupying `0.5%` of source width occupies about `1.25%` of a tile's width before detector resizing. Adapt geometry only after measuring recall, precision, and costs.
3. Crop each tile from the *upright original*, preserve aspect ratio, and downscale only when a tile exceeds the detector input cap. The example caps the tile's long side at `1600 px`; never upscale merely to reach the cap. If preprocessing introduces letterboxing, padding, or an additional crop, invert that transformation before interpreting the face as tile-local. Account for actual rounded pixel crop bounds when precision matters.
4. Execute `crop → detect → release` sequentially, retaining at most the current tile input in addition to the original/preview. On a callback-based SDK, keep the input alive until that request finishes, including after timeout/cancellation; do not start the next tile after cancellation. Gate every asynchronous result against the current image/session/operation, and emit the final response once.
5. Normalize each detection relative to *its input*, then restore tile detections to the source coordinate system. Android-style pixel boxes use `[left / inputWidth, top / inputHeight, right / inputWidth, bottom / inputHeight]`. For an iOS Vision-style bottom-left normalized box, first use `top = 1 - maxY`, `bottom = 1 - minY` (left/right unchanged). Reject non-finite values and non-positive area, clamp small out-of-range SDK values to `[0, 1]`, and only then map valid tile-local boxes into source coordinates:

   ```text
   sourceLeft   = tileLeft + localLeft   * tileWidth
   sourceTop    = tileTop  + localTop    * tileHeight
   sourceRight  = tileLeft + localRight  * tileWidth
   sourceBottom = tileTop  + localBottom * tileHeight
   ```

   Example: tile `(left=.6, top=.3, width=.4, height=.4)` and local box `(.25, .50, .75, 1.00)` yield source box `(.70, .50, .90, .70)`. Apply the minimum-face-width rule in the *detector input's* coordinate space; do not reapply its tile-local threshold after mapping to source width (a `0.01` tile width here corresponds to `0.004` of source width).
6. Merge all `PREVIEW`, `FULL`, and `TILE` candidates in source coordinates, **before** expanding boxes into editable regions. First reject tile-only candidates touching an *internal* tile edge unless supported by a duplicate from another tile or whole-image pass. Do not treat an actual source-image edge as an internal edge. Example boundary tolerance: `1%` of tile width or height.
7. Group overlapping candidates by transitive duplicate links, joining existing groups if a candidate connects them. **Example calibrated rule, not a universal threshold:** duplicates have `IoU >= .35`, or `intersection / smallerArea >= .60` **and** `centerDistance <= .35 * smallerBoxDiagonal`. The center constraint prevents merging nearby distinct faces; mere containment or overlap is insufficient. Select an *actual* box rather than averaging clipped detections. Example priority: `FULL > TILE > PREVIEW`; within a source prefer greater distance from tile edge, then greater area. Sort chosen boxes by `top`, `left`, then descending area before assigning IDs or emitting editor regions.
8. Enforce a single *whole-operation* deadline (the example uses `12 s`, not `12 s` per tile). Track successful passes separately from face count: success with zero faces is not a detector failure. A failed tile need not discard completed passes. On deadline, publish merged candidates from successful passes, even if empty; report detection failure only if **no pass succeeded**. Never apply stale results after cancellation, image replacement, or operation invalidation.

## Reproducible diagnostics

- Hand-check the mapping fixture above; also check a Vision local box `(minX=.25, minY=0, maxX=.75, maxY=.5)` maps to the *same* source box after the y-axis flip. Run equivalent fixtures through the actual adapters and assert finite, in-range, positive-area bounds under upright and EXIF-rotated input.
- On an annotated fixed image set, compare whole-image-only against whole-image-plus-tiles: no faces, one and many faces, face widths near `0.5%, 1%, 2%, 3%, 8%`, adjacent faces, overlapping tile edges, source edges, false-positive-prone backgrounds, all EXIF orientations, and the largest supported images. Confirm one output for a duplicated face, two outputs for close distinct faces, suppression of unsupported internal-edge fragments, and preservation of source-edge faces. Confirm profiles without tiling produce no tile inputs.
- Inject individual pass failures, an empty-but-successful pass, timeout, cancellation, and image replacement while inference is active. Observe a single completion, partial results only from successful passes, no stale editor update, and eventual release of the active native image after callback.
- Record face-size-bucket recall/precision, cold/warm latency p50/p95, peak RSS, and deadline rate per platform/device. Compare against a predeclared acceptance budget; build success alone is not quality evidence.

## Stop conditions

Stop rollout or revise measured tile geometry/merging if close faces collapse into one, boundary fragments survive, source-edge faces disappear, coordinate/orientation fixtures fail, stale callbacks alter another image, resources remain held, or precision/latency/memory budgets fail. Do not claim complete automatic detection; keep a user route to correct missed areas.
