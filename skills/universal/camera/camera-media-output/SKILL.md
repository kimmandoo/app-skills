---
name: camera-media-output
description: Use when capturing photos or videos to the platform media library with clear consent, recording indication, and reliable save/finalization handling.
---

# Camera media output

## Consent and capture contract

Before opening the camera, disclose what the app records, where media is stored or shared, and obtain the required user consent and platform permission. Show a clear visual and/or audible indication whenever the camera is in use or recording; do not rely on haptics, a permission prompt, or an invisible capture surface as the only indication. If camera access or data use is outside reasonable expectations, provide the prominent in-app disclosure and affirmative consent required by the applicable store policy. Preserve device-enforced shutter sounds, privacy indicators, and regional requirements rather than selecting APIs or output paths to evade them.

## Capture and publish

1. Request camera and media permissions appropriate to the feature. Request microphone access only if audio is included and disclosed; an intentionally audio-free video still needs clear recording indication and consent. Denial or interruption must produce a visible, accessible error.
2. Choose normal, supported capture APIs for the required quality and product behavior (for example, CameraX `ImageCapture`/`VideoCapture` or AVFoundation photo/video outputs on those platforms). Define the still and video resolution policies. Apply image orientation once (see [camera-focus-orientation](../camera-focus-orientation/SKILL.md)); do not suppress required audio or visual capture cues.
3. Publish to the destination the product disclosed to the user, with the correct media type and permissions. If using the system library, Android can save through MediaStore with the correct MIME type and collection; iOS can write finalized media through Photos' add-only flow. For an iOS video staged in a temporary file before a Photos write, retain it until the completion callback, then remove it after success or failure once cleanup is safe. For other destinations or stacks, retain temporary media until publication completes and avoid exposing partial output as a finished capture.
4. Surface separate events for capture requested, recording started, recording stopped, video finalized, and destination publication succeeded/failed. A stop request is not proof of durable media. Clean up partial outputs after errors and use collision-resistant file names or identifiers.
5. On lifecycle stop, camera interruption, permission revocation, or consent withdrawal, stop capturing and finalize or explicitly fail in-flight media. Keep any capture indicator accurate until actual capture ends. Restore screen-awake state when the camera closes.

## Verification

On each target physical device, verify consent/permission flow, clear indication throughout camera use and recording, published media at its disclosed destination, and decoded output after rapid photos, each supported lens, recording release, backgrounding, low storage, denial, and interruption. Check playable video, expected audio track, image orientation and resolution, and absence of partial entries after failure. Check applicable platform and regional requirements on target devices; a source scan or simulator build is not compliance evidence.

Policy references: [Apple App Review Guidelines §2.5.14](https://developer.apple.com/app-store/review/guidelines/) and [Google Play User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311). Consult current requirements for every distribution region.
