---
name: camera-focus-orientation
description: Use when implementing tap-to-focus camera capture or fixing photos and videos saved out of focus, rotated twice, or mirrored incorrectly.
---

# Camera focus and orientation

## Coordinate contract

Define the touch point in normalized top-left preview-view coordinates. Map it to the active camera's sensor/metering space after accounting for preview crop, view size, device orientation, and front-camera mirroring if present. Never feed raw view pixels to a sensor API with a different origin or orientation. Use the camera API's mapping where available (for example, Android's `DisplayOrientedMeteringPointFactory` or AVFoundation's focus/exposure points of interest); validate the chosen mapping against the visible preview and actual saved images.

## Serialize metering and capture

1. Request focus and exposure at the mapped point; optionally white balance when supported. Keep request identity so a second tap, lens switch, close, or session interruption invalidates the first pending capture. Validate access and camera readiness before capturing.
2. Wait for AF/AE completion, convergence, or a documented bounded timeout **before** capturing. Use completion/status signals provided by the camera API: for example, Android CameraX exposes the focus-and-metering future, while iOS can observe adjusting-focus/exposure flags across consecutive polls. Do not infer convergence from the first post-tap frame or a single quiet poll; a bounded fallback must be reported as a fallback rather than guaranteed focus.
3. On failure, cancelled request, timeout, or missing device, resolve the pending photo once with the product's chosen behavior (capture with a focus warning or fail); never leave it armed for a later unrelated frame.

## Apply orientation once

- Set still and video output orientation to match intended playback and the visible preview. Choose **one** still-image strategy: rotate pixels during encoding or store the correct orientation metadata, according to the supported capture API. Do not rotate or mirror a photo twice; configure video orientation separately, including any front-camera mirror policy.
- Refresh output orientation when the display or camera orientation changes. For example, CameraX can update `ImageCapture.targetRotation` and `VideoCapture.targetRotation`; iOS still capture can use orientation metadata or pixel rotation. Treat these as API-specific examples, not additional processing to apply together.
- For face-up/down or unknown orientation, preserve the last meaningful device orientation. Recheck each supported lens, all four physical orientations where applicable, and both photos and videos after changing any rotation or mirror mapping.

## Checks and stop conditions

On each target physical device, tap near center and all edges, capture moving/low-light subjects, switch lenses during/after focusing if supported, rotate before and during capture, and inspect **decoded saved pixels** plus metadata and video playback in more than one viewer. Verify the intended subject comes into focus after convergence, the tapped region maps correctly, supported cameras save upright stills, and video does not appear flipped. Reproduce a known AF race or double-rotation case before changing it and repeat the same scenario after. A simulator without camera hardware cannot prove these behaviors.
