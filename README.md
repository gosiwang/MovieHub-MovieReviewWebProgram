

---

# 🎬 MovieHub (Front-end)

> **기간:** 2025-10-20 ~ 2025-10-24 (Asia/Seoul)
> ※ 실제 작업 기간이 다르면 이 줄만 수정하세요.

## 📌 한 줄 소개

TMDB 오픈 API와 **Next.js(App Router)** 를 사용해 만든 영화 정보/검색/리뷰/추천 서비스.
**로컬 로그인 상태 관리**, **마이페이지(프로필/비밀번호/리뷰)**, **Q&A/공지**, **관리자 전용 UI(Design System 토큰화)**, **Gemini 기반 챗봇**까지 프론트에서 풀플로우를 구현했습니다.

---

## ✨ 핵심 기능(Feature Highlights)

* **홈/카테고리:** TMDB API로 인기/상영/평점 TOP 등 섹션 렌더링 (`app/mainpage/page.js`, `component/MovieSection.jsx`)
* **영화 상세:** 출연/제작진, 리뷰, 관련 정보 분리 컴포넌트 (`app/movieInfo/[id]/*`)
* **검색:** 쿼리 파라미터 기반 결과 페이지 (`app/search/page.js`)
* **인물 상세:** 인물 프로필·출연작 (`app/person/[id]/page.js`)
* **리뷰 작성/수정:** 팀 공통 **스타일 가이드(tokens)** 적용 (`app/review/write/page.js`, `app/review/edit/page.js`)
* **Q&A/공지:** 사용자 Q&A, 공지 리스트/작성(더미 데이터 연결) (`app/qna/*`, `app/notice/page.js`, `lib/data/*`)
* **마이페이지:** 프로필 편집/비밀번호 변경/회원 탈퇴/내 리뷰 확인 (`app/mypage/*`)
* **인증 상태 관리:** `AuthContext` 기반 로컬 로그인 상태 저장/복원 (`app/auth/AuthContext.js`)
* **관리자 영역:** 공통 레이아웃 + **관리자 전용 Design System 토큰** (`app/admin/_components/AdminLayout.js`, `app/admin/_lib/style/adminTokens.js`)
* **영화 추천 챗봇:** **Google Gemini** + 대화 히스토리 컨텍스트 유지 (`app/api/chatbot/route.js`, `component/Chatbot/`)
* **공통 헤더/푸터:** 로그인/검색/내비게이션 (`component/Header.js`, `component/Footer.js`)

---

## 🧱 기술 스택

* **Framework:** Next.js (App Router)
* **Lang/Runtime:** React 18, Node.js (LTS 권장)
* **Styles:** CSS Modules, 커스텀 토큰(`lib/style/styles.js`, `adminTokens.js`)
* **HTTP:** fetch + `axios` 래퍼(`apis/Api.js`)
* **외부 API:** TMDB(Open API), Google Gemini(API 라우트)
* **상태 관리:** React Context (`AuthContext`), 로컬 스토리지
* **폴더 구조:** App Router(페이지/라우팅), 컴포넌트/스타일/데이터 모듈화

---

## 📂 폴더 트리(핵심만)

```
src/
├─ apis/
│  └─ Api.js                      # axios 요청 헬퍼 (도메인 기반)
├─ app/
│  ├─ layout.js / globals.css     # 앱 공통 레이아웃
│  ├─ page.js                     # 루트(Home) → MainPage 위임
│  ├─ mainpage/page.js            # 메인 섹션(인기/상영중/평점 등)
│  ├─ movieInfo/[id]/             # 상세/크루/리뷰 리스트
│  ├─ person/[id]/page.js         # 인물 상세
│  ├─ search/page.js              # 검색
│  ├─ review/write|edit/          # 리뷰 작성/수정
│  ├─ qna/ ...                    # Q&A 목록/작성
│  ├─ notice/ ...                 # 공지
│  ├─ recommendations/page.js     # 추천(페이지 껍데기)
│  ├─ mypage/ ...                 # 프로필/비번변경/탈퇴/내리뷰
│  ├─ admin/
│  │  ├─ page.js                  # 관리자 진입
│  │  ├─ users/page.js            # 회원 관리 (UI)
│  │  ├─ fqa/page.js              # FAQ 관리 (UI)
│  │  ├─ notice/create/page.js    # 공지 작성 (UI)
│  │  └─ _components/AdminLayout.js
│  │     _lib/style/adminTokens.js# 관리자 전용 디자인 토큰
│  ├─ auth/
│  │  ├─ AuthContext.js           # 로그인 상태 저장/복원
│  │  └─ page.js                  # (선택) 인증 관련 페이지
│  ├─ login/page.js               # 로그인 화면
│  ├─ api/chatbot/route.js        # Gemini 프록시 API(서버 라우트)
│  └─ modal/...                   # 약관/개인정보 모달
├─ component/
│  ├─ Header.js / Footer.js       # 공통 레이아웃 구성요소
│  ├─ MovieCard.jsx               # 카드형 UI
│  ├─ MovieSection.jsx            # 섹션 컴포넌트
│  ├─ ReviewButton.js             # 공통 버튼
│  └─ Chatbot/index.js            # 플로팅 챗봇 UI
├─ lib/
│  ├─ style/styles.js             # 전역 토큰(색상/타이포/간격 등)
│  └─ data/*.js                   # 더미 데이터(공지/QnA/리뷰/회원 등)
└─ test/test.js                   # (샘플)
```

---

## 🔐 환경 변수(.env.local)

> **절대 깃에 올리지 말 것!** (면접 시는 키값 마스킹하고 사용 방식만 설명)

```bash
# TMDB API 키 (클라이언트 사용)
NEXT_PUBLIC_TMDB_API_KEY=YOUR_TMDB_KEY

# Gemini 서버 라우트에서 사용
GOOGLE_GEMINI_API_KEY=YOUR_GEMINI_KEY

# axios 기본 도메인 (Api.js가 이 값을 읽음)
# 현재 소스는 변수명이 `NEXT_PUBLIC`로 되어 있음 → 혼동 방지를 위해 아래처럼 변경 권장
NEXT_PUBLIC_BASE_URL=https://api.example.com
```

> 📎 **소스 개선 권장**: `apis/Api.js` 의 `DOMAIN = process.env.NEXT_PUBLIC` →
> `process.env.NEXT_PUBLIC_BASE_URL` 로 변수명 명확화.

---

## 🚀 실행 방법 (Getting Started)

```bash
# 1) Node.js LTS 설치 (v18+ 권장)

# 2) 의존성 설치
npm install

# 3) 개발 서버
npm run dev
# http://localhost:3000

# 4) 프로덕션 빌드/실행
npm run build
npm start
```

---

## 🧭 주요 라우팅 표

| 경로                       | 설명                   |
| ------------------------ | -------------------- |
| `/`                      | 홈(MainPage 섹션 렌더링)   |
| `/search?q=...`          | 검색 결과                |
| `/movieInfo/[id]`        | 영화 상세/크루/리뷰          |
| `/person/[id]`           | 인물 상세                |
| `/review/write?movieId=` | 리뷰 작성                |
| `/review/edit?reviewId=` | 리뷰 수정                |
| `/mypage`                | 내 정보/설정 허브           |
| `/mypage/profileEdit`    | 프로필 편집               |
| `/mypage/changePwd`      | 비밀번호 변경              |
| `/mypage/myReview`       | 내 리뷰                 |
| `/mypage/withdraw`       | 회원 탈퇴                |
| `/qna` · `/qna/write`    | Q&A 목록/작성            |
| `/notice`                | 공지 목록                |
| `/admin`                 | 관리자 대시보드(레이아웃)       |
| `/admin/users`           | 관리자 – 회원             |
| `/admin/fqa`             | 관리자 – FAQ            |
| `/admin/notice/create`   | 관리자 – 공지 작성          |
| `/api/chatbot`           | Gemini 챗봇 API (POST) |

---

## 🧰 설계 포인트(면접관 포인트)

### 1) App Router & 구조화

* **기능 단위 디렉토리**(`app/*/page.js`), **세부 컴포넌트 분리**로 유지보수성 향상
* 공통 레이아웃(`layout.js`)과 **헤더/푸터/챗봇**을 전역에 배치 → 일관된 UX

### 2) 디자인 시스템 토큰화

* `lib/style/styles.js`, `admin/_lib/style/adminTokens.js`
  → 색상/타이포/간격/둥근 정도/쉐도우/트랜지션 **일원화**
  → **일관성 + 재사용성 + 다크/브랜드 확장 용이**

### 3) 상태 & 인증

* `AuthContext`로 **로그인 상태 저장/복원**(localStorage)
  → 새로고침/세션 간 **사용자 경험 유지**

### 4) API 경계 및 보안

* 클라이언트 노출이 필요한 키는 `NEXT_PUBLIC_*`,
  비공개 키는 **서버 라우트**(`app/api/chatbot/route.js`)에서만 사용
* API 호출 전용 헬퍼(`apis/Api.js`)로 **에러/로깅 일원화**

### 5) 챗봇(대화 히스토리)

* `history` 배열을 수신해 이전 발화를 컨텍스트로 전달
  → **맥락 유지 대화**로 추천 품질 개선

### 6) 더미 데이터 → 실제 연동 용이

* `lib/data/*.js`에 더미 분리
  → **후속 백엔드 API로 치환** 시 영향 최소화

---

## 🧪 품질/개선 아이디어

* **폴더/변수 정리**

  * `NEXT_PUBLIC` 변수명 → `NEXT_PUBLIC_BASE_URL` 로 개선
  * `component` vs `components` 폴더명 통일
* **에러/로딩 상태 공통 컴포넌트화**

  * 스피너/에러 바운더리/토스트
* **접근성(A11y)**

  * 시맨틱 태그, 키보드 포커스 스타일, ARIA 속성
* **성능**

  * `next/image` 적극 활용(이미지 최적화)
  * 캐시/ISR(`revalidate`) 전략 명확화
  * 코드 스플리팅(동적 import) 적용 범위 확대
* **테스트**

  * 유닛 테스트(Jest/RTL) 스캐폴딩
  * 통합 테스트(Playwright) 시나리오: 로그인→검색→리뷰 작성
* **CI/CD**

  * PR 템플릿/ESLint/Prettier/Type Check 추가
  * Vercel or Docker 배포 파이프라인

---

## 🔑 보안/운영 체크리스트

* [ ] `.env.local`은 절대 커밋 금지
* [ ] API 키는 **권한 제한**(TMDB Read, Gemini 최소 권한)
* [ ] 로깅 시 민감정보 필터링
* [ ] 에러 페이지(404/500) 및 Fallback 준비
* [ ] 외부 의존 API 장애 대비(리트라이/타임아웃/대체 메시지)

---

## 🖥️ 스크립트(예시)

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

> 실제 `package.json`과 다르면 이 블록만 맞춰서 수정하세요.

---

## 🗂️ 샘플 코드 스니펫

### TMDB 섹션 렌더링

```js
// app/mainpage/page.js
import MovieSection from "@/component/MovieSection";

async function fetchMovies(category) {
  const API_KEY = process.env.NEXT_PUBLIC_TMDB_API_KEY;
  const res = await fetch(
    `https://api.themoviedb.org/3/movie/${category}?api_key=${API_KEY}&language=ko-KR`,
    { next: { revalidate: 3600 } }
  );
  return res.json();
}

export default async function MainPage() {
  const [popular, topRated] = await Promise.all([
    fetchMovies("popular"),
    fetchMovies("top_rated"),
  ]);
  return (
    <>
      <MovieSection title="인기" movies={popular.results} />
      <MovieSection title="평점 TOP" movies={topRated.results} />
    </>
  );
}
```

### Gemini 챗봇 API (대화 히스토리)

```js
// app/api/chatbot/route.js (발췌)
export async function POST(request) {
  const { message, history = [] } = await request.json();
  const GEMINI_API_KEY = process.env.GOOGLE_GEMINI_API_KEY;
  if (!GEMINI_API_KEY) {
    return NextResponse.json({ error: "API key missing" }, { status: 500 });
  }
  // ... Gemini 호출 로직 (history 포함)
}
```

---

## 🙋‍♂️ 본인 기여 포인트 (면접용 정리)

* **요구사항 정의 → 정보설계 → UI/컴포넌트 설계 → 개발 → 테스트 → 문서화** 전 과정 주도
* **Design System 토큰화**로 일관된 UI/UX와 생산성 확보
* **App Router 기반 구조화**로 페이지/기능 간 결합도 ↓, 재사용성 ↑
* **로컬 인증 상태/마이페이지/리뷰 플로우** 등 사용자 시나리오 완성
* **Gemini 챗봇** 프록시 라우트로 키 보안 + 맥락 대화 구현
* **더미 데이터**로 백엔드 연동 이전 단계에서도 UX 검증 가능하도록 설계

---

## 📈 향후 로드맵

* 백엔드 연동(회원/리뷰/Q&A/공지 실데이터)
* 소셜 로그인(OAuth)·JWT 세션 관리
* 개인화 추천(장르/시청 이력 기반)
* 다크모드 토큰 확장, 관리자 권한 가드
* Lighthouse 90+ 목표 최적화

---

## 📝 라이선스

사내/개인 프로젝트 정책에 맞춰 설정하십시오. (예: MIT)

---

### 부록: 데모 영상/이미지

* Git LFS 제한 때문에 **영상은 Git에 직접 올리지 말고** YouTube 비공개 링크 또는 GitHub Release/Issue 첨부 권장
* README에는 **썸네일 이미지 2~3장**과 유튜브 링크로 연결

---


