# prayer-legal

「직장인 말씀&기도」(Worker: Word&Prayer) 앱의 이용약관·개인정보처리방침을 GitHub Pages로 공개하기 위한 전용 저장소입니다. 앱 소스 코드는 포함하지 않습니다.

이 저장소는 **public**이어야 GitHub Pages가 무료로 동작합니다.

## 문서 위치

- `docs/index.html` — 문서 목록
- `docs/legal/terms.html` / `privacy.html` — 한국어
- `docs/legal/terms.en.html` / `privacy.en.html` — English
- `docs/legal/data-safety-summary.md` — Play Console "데이터 보안" 양식 작성용 요약

각 HTML 파일 상단에 `출시 전 법률 검토 필요` 주석이 있습니다. 실제 법률 자문 없이 그대로 게시하지 마세요.

## GitHub Pages 설정 절차

1. 이 저장소를 GitHub에 새로 만들고(예: `prayer-legal`), **Public**으로 설정한 뒤 이 폴더 내용을 push합니다.
   ```
   git remote add origin https://github.com/<계정>/prayer-legal.git
   git branch -M main
   git push -u origin main
   ```
2. GitHub 저장소 페이지에서 **Settings → Pages**로 이동합니다.
3. **Build and deployment → Source**를 `Deploy from a branch`로 선택합니다.
4. **Branch**를 `main`, 폴더를 `/docs`로 선택하고 **Save**합니다.
5. 1~2분 후 상단에 발행된 URL이 표시됩니다. 형식: `https://<계정>.github.io/prayer-legal/`

## 최종 URL

| 문서 | URL |
|---|---|
| 문서 목록 | `https://<계정>.github.io/prayer-legal/` |
| 이용약관 (ko) | `https://<계정>.github.io/prayer-legal/legal/terms.html` |
| 개인정보처리방침 (ko) | `https://<계정>.github.io/prayer-legal/legal/privacy.html` |
| Terms of Service (en) | `https://<계정>.github.io/prayer-legal/legal/terms.en.html` |
| Privacy Policy (en) | `https://<계정>.github.io/prayer-legal/legal/privacy.en.html` |

`<계정>`과 최종 URL은 저장소 생성 후 확정되는 대로 이 표를 갱신합니다.

## 내용 업데이트 시

앱의 실제 동작(가격, 무료/유료 기능 경계, 사용 SDK)이 바뀌면 이 문서도 함께 갱신해야 합니다. 특히:
- 구독 가격이 바뀌면 `terms.html`/`terms.en.html`의 ④ 항목
- 분석·SDK 구성이 바뀌면 `privacy.html`/`privacy.en.html`의 ⑤ 항목과 `data-safety-summary.md`
