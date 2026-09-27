<!-- 출시 전 법률/정책 검토 필요 — 이 문서는 App Store Connect "앱 개인정보 보호" 설문에 그대로 옮기기 위한 실무 초안이며 법률 자문이 아닙니다. -->

# App Store Connect "앱 개인정보 보호" 답변 요약 (iOS 전용)

작성 기준: 2026-09-27, `prayer-iOS` 코드베이스 전수 확인 결과 기반.
안드로이드 쪽 답변은 [`data-safety-summary.md`](data-safety-summary.md) 에 따로 있다. **두 앱의 구성이 다르므로 서로 베끼지 않는다.**

## 확인된 사실

- 서버가 없다. 이용자가 만드는 모든 기록(기도제목·묵상 노트·진도·스트릭·설정)은 기기 안에만 저장된다(SwiftData + `UserDefaults`). 기기 밖으로 보내는 코드 경로가 없다.
- **외부 패키지가 0개다.** `Prayer.xcodeproj` 에 원격 Swift Package 참조가 하나도 없고, `URLSession` 을 쓰는 코드도 없다. 안드로이드가 쓰는 **RevenueCat 은 iOS 에 없다** — 결제는 StoreKit 2 로 직접 구현했다.
- `Analytics` 는 `#if DEBUG` 에서 `print` 만 하는 자리표시자다. 분석 SDK가 아니며 아무것도 전송하지 않는다.
- 네트워크를 타는 것은 두 가지뿐이고, 둘 다 Apple 이 처리한다: ① StoreKit 구독 구매·복원 ② 약관·방침 페이지를 여는 `SFSafariViewController`.
- 광고 없음. IDFA(App Tracking Transparency) 사용 없음.

## 설문 답변

### "이 앱이 데이터를 수집합니까?" → **아니요 (Data Not Collected)**

개발자와 제3자 파트너가 앱에서 수집하는 데이터가 없다. 그러므로 데이터 유형 표는 비워 둔다.

> 결제 과정에서 Apple 이 자체적으로 처리하는 정보는 Apple 이 자기 정책에 따라 다루는 것이고,
> 개발자가 수집하는 것이 아니므로 이 설문에 적지 않는다(Apple 의 안내와 같다).
> 안드로이드에서 "예"로 답한 이유는 RevenueCat 때문인데, iOS 에는 그 SDK가 없다.

### 추적(Tracking)

- 다른 회사의 앱·웹사이트를 가로지르는 추적: **없음**
- `NSPrivacyTracking`: `false`, `NSPrivacyTrackingDomains`: 비어 있음

## 개인정보 보호 매니페스트 (`PrivacyInfo.xcprivacy`)

`prayer-iOS` 의 `Prayer/Resources/PrivacyInfo.xcprivacy` 에 있고 빌드 시 번들에 들어간다.

| 항목 | 값 | 근거 |
|---|---|---|
| `NSPrivacyTracking` | `false` | 추적 없음 |
| `NSPrivacyTrackingDomains` | 없음 | 추적 도메인 없음 |
| `NSPrivacyCollectedDataTypes` | 없음 | 수집하는 데이터 없음 |
| `NSPrivacyAccessedAPICategoryUserDefaults` | 사유 `CA92.1` | `AppPreferences` 가 이 앱 자신의 설정(`user_prefs`)만 읽고 쓴다 |

`UserDefaults` 는 Apple 이 사유 선언을 요구하는 API 라, 매니페스트가 없으면 업로드 때 경고를 받는다.
다른 사유 선언 대상 API(파일 타임스탬프·디스크 여유 공간·시스템 부팅 시각·활성 키보드)는 쓰지 않는다.

## 데이터 삭제

서버에 보관하는 데이터가 없으므로 별도 요청 절차가 필요 없다. 이용자가 설정의 "처음부터 다시 설정"
또는 앱 삭제로 즉시·완전히 지울 수 있다.

- 문의: breezeholicmall@gmail.com
