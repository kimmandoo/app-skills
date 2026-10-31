# Skill routing evaluation

Use these fixtures to review changes to skill descriptions and the README routing guide. They are examples, not measured results or a keyword-based router. The expected entry is the first skill to consult; a different skill can follow when the task crosses domains. Paths are relative to the repository root, and names correspond to `skills/**/SKILL.md` frontmatter names.

## Procedure

1. For each prompt, use the same loader, available skill descriptions, model and settings as the real integration. Ask it to select its starting skill without showing it the expected or avoid columns. Record the selected name, rationale, and any follow-up skills.
2. Compare the first selection with **Expected**. Count exact matches and report matches / total, missing or invalid selections, and each mismatch. The **Avoid first** column is a plausible wrong starting point, not a ban on using that skill later.
3. Record the model, loader, date, revision, prompt, actual selection and outcome. Repeat the same cases and settings before and after a change; do not claim an improvement without both results. Review failures individually, especially ambiguous cases, and update a fixture only with a reason.
4. Routing alone does not evaluate whether the skill's instructions are followed or whether the app works. Follow with task-specific behavioral scenarios and actual verification where applicable.

## Fixtures

| ID | User request | Expected | Avoid first | Why |
| --- | --- | --- | --- | --- |
| R01 | APK 빌드는 성공했는데 바뀐 화면이 기기에서 열리는지 확인해줘. | device-verification | android-build | The requested evidence is a real run, not another build. |
| R02 | Android AAB 릴리스 서명 오류를 고쳐줘. | android-build | google-play-submission | Signing fails before Play upload. |
| R03 | Play Console 업로드 후 심사 거절 사유를 처리해줘. | google-play-submission | android-build | Console review is the task. |
| R04 | Compose 화면에서 불필요한 recomposition을 줄여줘. | compose-recomposition | mobile-ui-ux-verification | The bottleneck is toolkit-specific. |
| R05 | Xcode에서 iOS 앱 프로비저닝 오류가 나. | ios-build | app-store-submission | Provisioning is an iOS build issue. |
| R06 | App Store Connect에서 심사 거절된 빌드를 다시 제출해줘. | app-store-submission | ios-build | The immediate task is store review. |
| R07 | iOS 앱의 추적 권한 문구 때문에 심사에서 거절됐어. | att-review | app-store-submission | Tracking-consent finding has a specialized workflow. |
| R08 | Flutter iOS 릴리스 빌드와 서명을 준비해줘. | flutter-mobile-build | ios-build | The build starts in the Flutter project. |
| R09 | KMP에서 공통 코드와 iOS 네이티브 코드 경계를 정해줘. | kmp-platform-boundaries | ios-build | The question concerns shared/native architecture. |
| R10 | 앱의 표시 버전과 Android 업로드 번호를 올려줘. | version-release | release-artifacts | Versioning comes before artifact production. |
| R11 | 배포 전에 빠진 권한과 개인정보 차단 요인을 점검해줘. | release-preflight | release-artifacts | The request is a pre-release blocker review. |
| R12 | 이미 만든 서명된 APK의 인증서와 버전 메타데이터를 확인해줘. | release-artifacts | release-preflight | The target is artifact inspection. |
| R13 | 앱스토어 소개 문구를 한국어로 현지화해줘. | store-submission-content | mobile-localization | Store copy is separate from in-app strings. |
| R14 | 앱 내 설정 화면의 언어를 한국어로 번역해줘. | mobile-localization | store-submission-content | This is in-app UI text. |
| R15 | 오프라인에서 수정한 이벤트들이 동시 수정으로 충돌해. | local-first-event-sync | offline-data-integrity | Event replay and conflicts are the primary issue. |
| R16 | 오프라인 백업 파일을 가져올 때 손상 여부를 검증해줘. | offline-data-integrity | local-first-event-sync | The task is file import integrity, not event sync. |
| R17 | 공유 컬렉션에서 초대받은 사용자의 접근 권한을 확인해줘. | shared-cloud-access | local-first-event-sync | Membership and authorization are primary. |
| R18 | 카메라에서 핀치 줌과 촬영 세션 상태가 충돌해. | camera-gesture-session | mobile-gesture-interactions | This involves camera session state. |
| R19 | 카메라 탭 초점 위치와 저장 사진 회전이 틀려. | camera-focus-orientation | camera-media-output | Focus and orientation are primary. |
| R20 | 녹화가 끝난 후 동영상 파일이 저장되지 않아. | camera-media-output | camera-gesture-session | Finalization and destination saving are primary. |
| R21 | 작은 얼굴을 큰 이미지에서 놓치는데 타일로 나눠 검출해줘. | tiled-face-detection | face-detector-selection | The requested technique is tiled detection. |
| R22 | 기기에서 정확도와 속도를 측정해 얼굴 검출기를 선택해줘. | face-detector-selection | tiled-face-detection | The task is detector selection. |
| R23 | 회원가입 과정에서 사용자가 이탈하는 지점을 개선해줘. | mobile-ux-improvement | mobile-ui-ux-verification | The goal is reducing friction in a task. |
| R24 | 화면의 빈 상태, 오류 상태, 접근성을 점검해줘. | mobile-ui-ux-verification | mobile-ux-improvement | The immediate request is a UI state audit. |
| R25 | 폴더블 기기에서 접힌 상태와 분할 화면을 검증해줘. | adaptive-window-posture | mobile-ui-ux-verification | Posture and multiwindow require a focused workflow. |
| R26 | 목록 스크롤과 카드 드래그 제스처가 서로 충돌해. | mobile-gesture-interactions | camera-gesture-session | This is general UI gesture arbitration. |
| R27 | The Android Gradle build cannot find the required SDK. | android-build | device-verification | The build environment is failing before a device run. |
| R28 | The built iOS app crashes on device when opening the changed flow; verify it. | device-verification | ios-build | The request is to exercise a built app. |
| R29 | Replaying the same remote event batch creates duplicate records. | local-first-event-sync | shared-cloud-access | Deduplication during event replay is the issue. |
| R30 | Translate the Play Store listing, not the app's UI. | store-submission-content | mobile-localization | The target is store listing content. |

## Result record template

- Date / repository revision:
- Loader, model and settings:
- Cases run / exact first-skill matches / invalid or missing selections:
- Per mismatch: case ID, selected skill, rationale, follow-up skills and notes:
- Limitations and next action:
