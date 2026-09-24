---
name: camera-focus-orientation
description: Use when implementing tap-to-focus camera capture or fixing photos and videos saved out of focus, rotated twice, or mirrored incorrectly.
---

# Camera focus and orientation

## Coordinate contract

Define the touch point in normalized top-left view coordinates. Map it to the active camera's sensor/metering space after accounting for preview crop, view size, device orientation, and front-camera mirroring. Never feed raw view pixels to a sensor API with a different origin or orientation. Android's `DisplayOrientedMeteringPointFactory` and AVFoundation's focus/exposure points of interest are platform-specific mappings; validate each against the visible preview and actual saved images.

## Serialize metering and capture

1. Request focus and exposure at the mapped point; optionally white balance when supported. Keep request identity so a second tap, lens switch, close, or session interruption invalidates the first pending capture. Validate access and camera readiness before capturing.
2. Wait for AF/AE completion, convergence, or a documented bounded timeout **before** capturing. Android CameraX exposes the focus-and-metering future; iOS can observe adjusting-focus/exposure flags across consecutive polls. Do not infer convergence from the first post-tap frame or a single quiet poll; a bounded fallback must be reported as a fallback rather than guaranteed focus.
3. On failure, cancelled request, timeout, or missing device, resolve the pending photo once with the product's chosen behavior (capture with a focus warning or fail); never leave it armed for a later unrelated frame.

## Apply orientation once

- Refresh CameraX `ImageCapture.targetRotation` and `VideoCapture.targetRotation` as the display or camera orientation changes; check the relationship to the visible preview and the saved result.
- On iOS, choose **one** still-image orientation strategy: rotate pixels during encoding or store the correct orientation metadata, according to the supported capture API. Do not rotate or mirror a photo twice. Configure video orientation separately to match intended playback and front-camera mirror policy.
- For face-up/down or unknown orientation, preserve the last meaningful device orientation. Recheck front/back, all four physical orientations, and both photos and videos after changing any rotation or mirror mapping.

## Checks and stop conditions

On supported physical devices, tap near center and all edges, capture moving/low-light subjects, switch lenses during/after focusing, rotate before and during capture, and inspect **decoded saved pixels** plus metadata and video playback in more than one viewer. Verify the intended subject comes into focus after convergence, the tapped region maps correctly, both cameras save upright stills, and video does not appear flipped. Reproduce a known AF race or double-rotation case before changing it and repeat the same scenario after. Stop if a tap can arm a stale capture, a photo is taken before metering settles, or the same orientation is applied twice. A simulator build without camera hardware cannot prove these behaviors.

