<!-- 출시 전 법률/정책 검토 필요 — 이 문서는 Play Console "데이터 보안" 양식에 그대로 옮기기 위한 실무 초안이며 법률 자문이 아닙니다. -->

# Google Play Console "데이터 보안" 양식 작성 요약

작성 기준: 2026-07-31, 코드베이스 전수 확인 결과 (build.gradle.kts / libs.versions.toml / AndroidManifest.xml / merged manifest / 소스 grep) 기반.

## 확인된 사실

- 앱 자체는 서버가 없고, 이용자가 작성하는 모든 콘텐츠(기도제목·묵상 노트·진도·설정)는 기기 로컬(Room DB + DataStore)에만 저장된다. 앱 코드가 이 데이터를 네트워크로 전송하는 경로는 없다.
- 앱에 포함된 외부 SDK는 **RevenueCat**(`com.revenuecat.purchases:purchases`, Google Play Billing 연동) 단 하나다. Firebase Analytics/Crashlytics, 광고 SDK, FCM 등은 포함되어 있지 않다(merged manifest에 `INTERNET`/`ACCESS_NETWORK_STATE`/`com.android.vending.BILLING` 권한이 전부 RevenueCat 의존성에서만 유입됨을 확인).
- 앱 내부 `AnalyticsRepository`는 실제 분석 SDK가 아니라 `Log.d`로 기기 로그에만 남기는 자리표시자로, 기기 밖으로 아무것도 보내지 않는다.

## Data safety 폼 입력값

### "데이터를 수집하거나 공유하나요?" → 예 (RevenueCat을 통해)

| 데이터 카테고리 | 세부 항목 | 수집 | 공유 | 용도 | 비고 |
|---|---|---|---|---|---|
| 금융 정보 | 구매 내역(purchase history) | 예 | 예 (RevenueCat) | 앱 기능(구독 상태 확인) | 카드번호 등 결제수단 정보 자체는 Google Play가 처리하며 앱·RevenueCat 모두 보관하지 않음 |
| 기기 또는 기타 ID | 앱 인스턴스 식별용 익명 ID | 예 | 예 (RevenueCat) | 앱 기능(구독 상태 확인) | 이름·이메일 등과 연결되지 않는 RevenueCat 발급 익명 ID |

위 두 항목 외에 개인 정보, 위치, 메시지, 사진/동영상, 오디오, 파일/문서, 캘린더, 연락처, 검색·탐색 기록, 건강 정보 등은 **수집하지 않음**으로 표기한다.

### 개별 항목 체크리스트

- 수집된 데이터는 암호화되어 전송되나요? → **예** (HTTPS)
- 이용자가 데이터 삭제를 요청할 수 있는 방법을 제공하나요? → **예** (아래 "삭제 요청 방법" 참고)
- 이 데이터는 필수인가요, 선택인가요? → 구독 기능 사용 시에만 발생(선택 — 무료 기능은 해당 없음)
- 데이터가 광고·마케팅 목적으로 사용되나요? → **아니요**
- 데이터가 이용자 프로필 생성에 사용되나요? → **아니요**

### 삭제 요청 방법 (양식의 "데이터 삭제 요청" 섹션에 기입)

1. 앱 자체 데이터(기도제목·묵상 노트·설정 등)는 서버에 보관되지 않으므로, 이용자가 설정의 "처음부터 다시 설정" 또는 앱 삭제로 즉시·완전히 삭제할 수 있다. 별도 요청 절차가 필요 없다.
2. RevenueCat이 보관하는 익명 구매 이력·앱 인스턴스 ID의 삭제를 원하는 이용자는 아래 이메일로 요청하면, 개발자가 RevenueCat 대시보드에서 해당 이용자 식별자(anonymous app user ID)의 삭제를 처리한다.
   - 문의: breezeholicmall@gmail.com

## 참고 링크

- RevenueCat 개인정보처리방침: https://www.revenuecat.com/privacy
- RevenueCat이 Data safety 폼 작성에 제공하는 가이드: https://www.revenuecat.com/docs/other/android-data-safety
