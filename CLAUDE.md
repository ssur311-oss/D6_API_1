# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 이 폴더는 무엇인가
바이브 코딩 수업 D6(10/21, "무료 API로 나만의 서비스 만들기")의 실습 폴더다. 빌드·린트·테스트 도구, 패키지, git 레포가 없다.
- `6일차 수업 자료/D06_1021.html` — 강사가 준 워크북(프롬프트 복사 + 검사 체크). 읽기 전용으로 다룬다.
- `6일차 수업 자료/d6-api/*.html` — 학생이 프롬프트로 만드는 API 화면(미션 A 날씨 `weather.html`, B 환율 `rate.html`, C 도시 검색 `city.html`, D 아침 대시보드 `morning.html`).
- `.claude/rules/api.md` — API 화면 규칙, API별로 확인한 사실, 실수에서 배운 규칙. **API 화면을 만들거나 고칠 때의 기준은 전부 여기에 있다.** 새 규칙도 여기에 추가한다.
- `.claude/skills/api-check/` — `/api-check <파일>` 스킬. 화면이 규칙을 지켰는지 **검사만** 하고 표로 보고한다(고치지 않음).

## 일하는 방식
- 커밋·푸시는 하지 말고, 커밋 메시지만 제안한다. 형식: `"[직군] D6: <미션> <이름>"`. (왜: 세이브 포인트는 학생이 검사한 뒤에 찍음)
- 학생은 비개발 직군이다. 설명과 화면 문구는 쉬운 한국어로 쓴다.

## 실행·확인 방법
- 실행: HTML 파일을 더블클릭(브라우저로 열기). 서버 불필요.
- API 응답 확인: `curl -s "<const API에 적힌 주소>"`로 실제 JSON 모양을 보고 나서 코드를 쓴다.
- 실패 화면 확인: 🔌 주소에서 글자 하나를 지우고 새로고침 → 안내 문구가 나오고 다른 카드는 그대로인지 본다.

## 화면 코드의 공통 구조
기존 파일들이 같은 뼈대를 쓴다. 새 화면도 맞춘다.
- `<head>`의 첫 `<script>`: `const API`(필요하면 화면에 보일 동네 이름 `PLACE`도 함께). 좌표·통화 같은 "주문 옵션"은 주소 문자열 안에 그대로 보이게 두어 학생이 손으로 바꿀 수 있게 한다.
- 색은 `:root` CSS 변수, 카드는 `.card`, 상태는 `.loading` / `.error` / `.api-box` 클래스.
- 본문 `<script>`: 응답 → 화면 변환 함수(render*)와 불러오기 함수(load*)를 나눈다. `morning.html`의 `getJson()` + `loadCard(boxId, apiName, what, render)`가 여러 카드용 기본 패턴이다.
- 주소를 동적으로 만드는 화면(`rate.html`, `city.html`)은 `API`에 고정 부분만 두고 뒤에 값을 붙인 뒤, 실제로 부른 주소를 API 상자에 갱신한다. 연속 입력 시 늦게 온 옛 응답이 덮어쓰지 않게 요청 번호(`searchId`)를 비교한다(`city.html`).
- API에서 온 문자열을 `innerHTML`에 넣을 때는 `esc()`로 감싼다.
- WMO 날씨 코드 → 한국어+이모지 표는 `weather.html`, `city.html`, `morning.html`에 같은 내용이 들어 있다. 고칠 때는 세 곳을 함께 고친다.
