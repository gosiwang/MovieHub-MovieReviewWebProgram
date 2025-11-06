## 🎬 MovieHub (Next.js 기반 영화 추천 & 리뷰 플랫폼)

* 📆 개발 기간: 2025.10.20 ~ 2025.10.24
* 👥 참여 인원: 6명 (Front-end 팀)
* 🧭 역할: 리뷰/추천/헤더/푸터/모달 UI 개발 및 사용자 경험 설계
> TMDB API를 활용한 영화 검색, 추천, 리뷰 서비스
> 사용자 맞춤 영화 추천, 자동완성 검색, 로그인/마이페이지 기능 포함

---

### 🧱 프로젝트 구조

```
src/
 ├─ app/
 │   ├─ page.js                # 메인 페이지 (영화 섹션별 리스트)
 │   ├─ recommendations/
 │   │    └─ page.js           # 장르별/기분별 맞춤 영화 추천
 │   ├─ modal/
 │   │    ├─ personal/page.js  # 개인정보 처리방침 모달
 │   │    └─ use/page.js       # 사이트 이용약관 모달
 │   ├─ auth/AuthContext.js    # 로그인/사용자 상태 관리
 │   ├─ mypage/                # 마이페이지
 │   └─ login/                 # 로그인 페이지
 │
 ├─ component/
 │   ├─ MovieSection.jsx       # 영화 섹션 렌더링 컴포넌트
 │   ├─ Header.jsx             # 상단 헤더 (검색 & 추천)
 │   ├─ Footer.jsx             # 하단 푸터 (정책/링크)
 │   └─ styles/
 │        ├─ Header.module.css
 │        └─ Footer.module.css
 │
 └─ public/
      └─ Logo.png
```

---

### 🚀 주요 기능 요약

#### 1️⃣ **메인 페이지 (`page.js`)**

* TMDB API를 통해 실시간 영화 데이터 Fetch
* 인기작 / 현재 상영작 / 평점 높은 영화 / 상영 예정작 4가지 섹션 구성
* 데이터 캐싱(1시간) 적용으로 성능 최적화
* `MovieSection` 컴포넌트를 재사용하여 UI 일관성 유지

```js
<MovieSection title="🔥 인기 영화" movies={popular} />
<MovieSection title="🎥 현재 상영작" movies={nowPlaying} />
<MovieSection title="⭐ 평점 높은 영화" movies={topRated} />
<MovieSection title="⏳ 상영 예정작" movies={upcoming} />
```

---

#### 2️⃣ **추천 페이지 (`recommendations/page.js`)**

* URL 쿼리(`?genres=18,10749&label=로맨스`)를 받아
  해당 장르의 영화를 TMDB `/discover/movie` API로 Fetch
* 장르별, 기분별, 상황별 추천 버튼과 연동
* MovieSection 컴포넌트 재활용으로 시각적 일관성 유지

```js
https://yourdomain.com/recommendations?genres=18,10749&label=로맨스
```

---

#### 3️⃣ **헤더 (`Header.jsx`)**

* ✅ 영화 검색 자동완성 (TMDB `/search/multi`)
* ✅ 최근 검색어 저장 / 삭제 / 저장 끄기 기능
* ✅ 장르·기분·상황별 추천 드롭다운 메뉴
* ✅ 로그인 / 로그아웃 / 마이페이지 이동
* ✅ 관리자 로그인 시 “관리자 페이지로 이동” 버튼 표시
* ✅ 반응형 UI 및 포커스 외부 클릭 시 자동 닫힘

---

#### 4️⃣ **푸터 (`Footer.jsx`)**

* MovieReview 브랜드 섹션
* 서비스 / 정보 / 소셜 링크
* 개인정보 처리방침, 이용약관 모달 연결
* TMDB 데이터 출처 명시
* 연도 자동 업데이트
* 완전한 반응형 CSS 적용 (`Footer.module.css`)

---

### 🌐 API 연동

모든 영화 데이터는 [The Movie Database (TMDB)](https://www.themoviedb.org/) API를 사용합니다.

`.env.local` 파일에 아래를 추가하세요:

```
NEXT_PUBLIC_TMDB_API_KEY=당신의_TMDB_API_KEY
```

---

### 🧩 환경 및 기술 스택

| 기술                           | 역할                          |
| ---------------------------- | --------------------------- |
| **Next.js 14+ (App Router)** | SSR & SSG, 캐싱               |
| **React Hooks**              | 상태관리, 비동기 요청 제어             |
| **TMDB API**                 | 영화 데이터 제공                   |
| **CSS Modules**              | 컴포넌트별 스타일링                  |
| **LocalStorage**             | 최근 검색어 및 로그인 정보 저장          |
| **useRouter / searchParams** | 클라이언트 내비게이션 및 추천페이지 파라미터 처리 |

---

### ⚙️ 실행 방법

```bash
# 1. 프로젝트 클론
git clone https://github.com/hyunn9799/sesac-movie-project.git
cd MovieHub

# 2. 패키지 설치
npm install

# 3. 환경변수 설정
cp .env.example .env.local
# NEXT_PUBLIC_TMDB_API_KEY 입력

# 4. 개발 서버 실행
npm run dev
```

👉 브라우저에서 [http://localhost:3000](http://localhost:3000) 열기

---

### 💡 추가 구현 포인트

* [ ] 영화 상세페이지 구현 예정 (`/movie/[id]`)
* [ ] 사용자 리뷰 등록/수정/삭제 기능 추가
* [ ] 다크모드 지원
* [ ] 즐겨찾기 / 찜 기능
* [ ] 관리자용 영화 통계 대시보드

---

### 📜 라이선스

> 본 프로젝트는 [TMDB API 사용정책](https://www.themoviedb.org/documentation/api/terms-of-use)에 따라
> 비상업적 학습/포트폴리오 목적의 사용만을 허용합니다.
>
> © {currentYear} MovieHub. All rights reserved.

