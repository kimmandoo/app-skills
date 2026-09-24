---
name: camera-media-output
description: Use when capturing photos or videos to the platform media library with clear consent, recording indication, and reliable save/finalization handling.
---

# Camera media output

## Consent and capture contract

Before opening the camera, disclose what the app records, where media is stored or shared, and obtain the required user consent and platform permission. Show a clear visual and/or audible indication whenever the camera is in use or recording; do not rely on haptics, a permission prompt, or an invisible capture surface as the only indication. If camera access or data use is outside reasonable expectations, provide the prominent in-app disclosure and affirmative consent required by the applicable store policy. Preserve device-enforced shutter sounds, privacy indicators, and regional requirements rather than selecting APIs or output paths to evade them.

## Capture and publish

1. Request camera and media permissions appropriate to the feature. Request microphone access only if audio is included and disclosed; an intentionally audio-free video still needs clear recording indication and consent. Denial or interruption must produce a visible, accessible error.
2. Choose normal, supported capture APIs for the required quality and product behavior, such as CameraX `ImageCapture`/`VideoCapture` or AVFoundation photo/video outputs. Define the still and video resolution policies. Apply image orientation once (see [camera-focus-orientation](../camera-focus-orientation/SKILL.md)); do not suppress required audio or visual capture cues.
3. On Android, save photo/video through MediaStore with the correct MIME type and collection. On iOS, write finalized photo/video using Photos' add-only flow if the product uses the system library. Keep a temporary video file until the Photos completion callback; remove it after success or failure once cleanup is safe.
4. Surface separate events for capture requested, recording started, recording stopped, video finalized, and library save succeeded/failed. A stop request is not proof of durable media. Clean up partial outputs after errors and use collision-resistant file names or identifiers.
5. On lifecycle stop, camera interruption, permission revocation, or consent withdrawal, stop capturing and finalize or explicitly fail in-flight media. Keep any capture indicator accurate until actual capture ends. Restore screen-awake state when the camera closes.

## Verification

On physical Android and iOS devices, verify consent/permission flow, clear indication throughout camera use and recording, captured library entries, and decoded media after rapid photos, front/back camera, recording release, backgrounding, low storage, denial, and interruption. Check playable video, expected audio track, image orientation and resolution, and absence of partial entries after failure. Check applicable platform and regional requirements on target devices; a source scan or simulator build is not compliance evidence.

Policy references: [Apple App Review Guidelines §2.5.14](https://developer.apple.com/app-store/review/guidelines/) and [Google Play User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311). Consult current requirements for every distribution region.
