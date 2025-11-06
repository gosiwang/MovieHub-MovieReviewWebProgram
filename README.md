# 🎬 MovieHub — 영화 정보 및 추천 서비스 (Front-end)

> 📆 개발 기간: 2025.10.20 ~ 2025.10.24
> 👥 참여 인원: 6명 (Front-end 팀)
> 🧭 역할: **리뷰/추천/헤더/푸터/모달 UI 개발 및 사용자 경험 설계**

---

## 📌 프로젝트 개요

**MovieHub**는 TMDB 오픈 API를 기반으로 한 영화 정보 및 추천 서비스입니다.
Next.js(App Router)와 React Context를 사용해 **검색 → 상세 → 리뷰 → 추천 → 마이페이지**까지
하나의 플로우로 연결된 사용자 경험을 제공합니다.

> 🎯 목표: “사용자가 한눈에 영화 정보를 탐색하고, 자신의 감정에 맞는 영화를 추천받을 수 있는 플랫폼 구현”

---

## 🧱 기술 스택

| 구분            | 사용 기술                           |
| ------------- | ------------------------------- |
| **Framework** | Next.js (App Router), React 18  |
| **언어/런타임**    | JavaScript (ES6+), Node.js      |
| **스타일링**      | CSS Modules, Custom Tokens      |
| **데이터/API**   | TMDB(Open API), Axios(fetch)    |
| **상태 관리**     | React Context, LocalStorage     |
| **배포/환경**     | Vercel, .env.local (API key 관리) |

---

## 🗂️ 전체 구조 요약

```
src/
├─ app/
│  ├─ mainpage/              # 메인 (인기/상영중/평점 등)
│  ├─ movieInfo/[id]/        # 영화 상세
│  ├─ search/                # 검색 결과
│  ├─ recommendations/       # 맞춤 추천
│  ├─ review/                # 리뷰 작성/수정
│  ├─ mypage/                # 프로필/비밀번호/내 리뷰
│  ├─ admin/                 # 관리자 UI
│  ├─ modal/                 # 정책/약관 모달
│  ├─ auth/                  # 로그인 상태 관리
│  └─ api/chatbot/           # Gemini 챗봇
├─ component/                # 공통 UI 컴포넌트
│  ├─ Header.js / Footer.js
│  ├─ MovieSection.jsx / MovieCard.jsx
│  └─ Chatbot/
├─ lib/
│  ├─ style/styles.js        # 디자인 토큰
│  └─ data/                  # 더미 데이터
└─ apis/Api.js               # axios 요청 헬퍼
```

---

## 💼 내가 담당한 주요 기능

### 1️⃣ **헤더(Header) & 푸터(Footer) UI / 기능 구현**

| 기능                 | 설명                                       |
| ------------------ | ---------------------------------------- |
| 🔍 **검색 기능**       | TMDB `search/multi` API를 활용한 실시간 검색 자동완성 |
| 🕹️ **추천 메뉴 드롭다운** | “기분/상황/장르”별 추천 페이지로 이동                   |
| 🧭 **라우팅 제어**      | Next.js App Router 기반 `Link` 네비게이션       |
| 💾 **최근 검색어 저장**   | LocalStorage 저장/삭제 기능 및 비활성화 토글          |
| 🧠 **로그인 상태 반영**   | `AuthContext`의 로그인 정보에 따라 메뉴 동적 표시       |
| 🧩 **푸터 모달 연결**    | “이용약관 / 개인정보 처리방침” 모달 직접 연결 및 제어         |

📁 관련 파일

```
src/component/Header.js  
src/component/Footer.js  
src/styles/Header.module.css  
src/styles/Footer.module.css  
src/app/modal/personal/page.js  
src/app/modal/use/page.js  
```

**핵심 구현 포인트**

* React 훅(`useState`, `useEffect`, `useRouter`) 기반 UI 제어
* TMDB API 호출 최적화 (`debounce` 방식으로 불필요 호출 방지)
* `localStorage` 동기화 로직 분리 (최근 검색어 저장/삭제 관리)
* 다중 모달 관리 구조 설계 (`ModalPortal` 방식 대신 페이지 라우팅으로 제어)

---

### 2️⃣ **추천(Recommendations) 페이지**

| 기능                  | 설명                                                |
| ------------------- | ------------------------------------------------- |
| 🎯 **감정/상황 기반 추천**  | `/discover/movie?with_genres=` API 호출             |
| 🧠 **Query 기반 필터링** | URL 파라미터(`?genres=18,10749&label=슬플 때`)에 따라 결과 표시 |
| 📄 **결과 리스트 UI**    | `<MovieCard />` 컴포넌트로 반복 렌더링                      |
| 🪶 **로딩/에러 핸들링**    | 데이터 없을 시 사용자 친화적 문구 표시                            |

📁 관련 파일

```
src/app/recommendations/page.js
src/component/MovieCard.jsx
src/component/MovieSection.jsx
```

**핵심 구현 포인트**

* 비동기 처리: `Promise.all` + `async/await`
* 캐싱 전략: `next: { revalidate: 3600 }` (ISR 방식)
* 장르별 필터링 로직 모듈화로 향후 확장성 확보

---

### 3️⃣ **모달 (약관 / 개인정보처리방침) 구조 설계**

* 페이지 라우팅 기반의 **비침투적 모달 구조**
* `/modal/personal`, `/modal/use` 두 페이지로 분리
* 모달 닫기 시 `router.back()` 으로 이전 상태 복원

📁 관련 파일

```
src/app/modal/personal/page.js
src/app/modal/use/page.js
```

---

## 🧠 기술적 설계 포인트

| 항목                         | 설명                    |
| -------------------------- | --------------------- |
| **1. 구조화된 App Router 사용**  | 기능별 폴더 분리로 유지보수 용이    |
| **2. 디자인 토큰화(styles.js)**  | 색상, 간격, 타이포, 쉐도우 일원화  |
| **3. Context 기반 인증 상태 유지** | 새로고침 시에도 로그인 유지       |
| **4. TMDB API와 fetch 결합**  | API Key 관리 및 캐싱 처리 분리 |
| **5. 반응형 디자인**             | 미디어쿼리 + Grid/Flex 병행  |

---

## ⚙️ 실행 방법

```bash
# 1. 의존성 설치
npm install

# 2. 환경 변수 설정
echo "NEXT_PUBLIC_TMDB_API_KEY=YOUR_TMDB_KEY" > .env.local

# 3. 개발 서버 실행
npm run dev
```

> 접속: [http://localhost:3000](http://localhost:3000)

---

## 🔑 면접/발표용 핵심 포인트

* ✅ TMDB API의 `search/multi`, `discover/movie`를 활용해
  **사용자 맞춤 추천**과 **실시간 검색** 구현
* ✅ App Router 기반 구조로 **페이지 단위 코드 분리 + 유지보수성 향상**
* ✅ `localStorage`를 이용한 **검색 기록 및 로그인 상태 저장**
* ✅ 모달/추천/검색 UI 간 **UX 플로우 통합 설계**
* ✅ 디자인 토큰 기반으로 **일관된 스타일 시스템 구성**

---

## 📈 향후 개선 방향

* 백엔드 API 연동 (회원/리뷰 실데이터)
* JWT 기반 인증 고도화
* 다크모드 및 접근성(A11y) 향상
* 코드 스플리팅 및 `Suspense` 기반 로딩 최적화

---

## 🪪 라이선스

MIT License

> 이 프로젝트는 TMDB의 공식 서비스가 아니며,
> TMDB에서 제공하는 오픈 API를 학습용으로 사용합니다.

---

