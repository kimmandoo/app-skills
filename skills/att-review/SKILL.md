---
name: att-review
description: Diagnose an iOS App Tracking Transparency review failure and collect evidence of the actual ATT, consent, and advertising sequence.
---

# ATT app review

## Gather evidence

Record the review submission/build, device/OS, review message, final `Info.plist`, actual SDK/data flows, App Store Connect App Privacy answers, review notes, and availability of a physical iPhone/iPad. Advertising consent is not Apple's ATT authorization. Do not infer tracking from advertising alone: inspect whether app/SDK data is linked with third-party app/web data for ad targeting/measurement or shared with a data broker.

## Check the runtime gate

1. If tracking occurs, confirm the built app contains `NSUserTrackingUsageDescription` and calls `ATTrackingManager.requestTrackingAuthorization` while active. Determine whether the ATT system prompt is reachable before any trackable SDK or data requests. If tracking does not occur, align SDK behavior, privacy manifests, and store answers rather than adding a misleading prompt.
2. Treat `notDetermined` (for example due to inactivity or dismissal) as pending; retry when active, not as authorized/denied. Gate any trackable SDK initialization/request on an actual ATT decision and on any separate consent obligations that apply to the product. If the app uses Google UMP, refresh information and present required forms at the appropriate point and check `canRequestAds` before requesting ads; do not mistake UMP consent for ATT. Denial must not disable core app functionality.
3. If a consent platform shows an IDFA pre-prompt before ATT or persists after denial, check cached consent state and console-configured copy; do not claim a binary patch changed server-managed messaging. Ensure sensitive app content and private file paths are not passed to ads/diagnostic SDKs unless specifically intended and disclosed.

## Reproduce and submit

On an unlocked physical test device with tracking requests allowed, reset the app's prior choice appropriately (fresh install/authorization reset); check Screen Time, managed-device, or child-account restrictions. Use the reviewed release/TestFlight configuration. Record uninterrupted fresh-launch footage: system ATT prompt, decision, any separate consent UI if used, and continued primary app flow after refusal. Redact private content, notifications, and identifiers. Simulator footage alone does not establish physical-device review behavior.

If a binary changes, use a new upload build number. Align App Privacy and review notes with the observed SDK behavior. Provide the reviewer precise reproduction steps and attach evidence only after capture; never state that device capture, upload, or store-console updates occurred without actually completing them. A tracking-enabled app must not evade a review issue by merely changing its App Privacy answer to No.

References: [Apple ATT API](https://developer.apple.com/documentation/apptrackingtransparency/attrackingmanager/requesttrackingauthorization(completionhandler:)), [Apple privacy rules](https://developer.apple.com/app-store/user-privacy-and-data-use/), [Google UMP](https://developers.google.com/admob/ios/privacy), [Google IDFA guidance](https://developers.google.com/admob/ios/privacy/idfa).
