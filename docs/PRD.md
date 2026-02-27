# 포트폴리오 사이트 PRD (Product Requirements Document)

## 1. 개요

### 1.1 프로젝트 목적

개발자(게임 개발 · AI) 포트폴리오를 전문적이고 세련되게 소개하는 정적 웹사이트를 구축한다.
Unity · Unreal · AI 프로젝트 갤러리, 개인 소개(About), 블로그 기능을 포함한다.

### 1.2 참고 사이트 분석

| 항목 | 참고 사이트 현황 | 개선 방향 |
|------|----------------|-----------|
| 홈 레이아웃 | 사이드바 카테고리 + 카드 그리드 | Hero 섹션 추가, 카드 디자인 고도화 |
| 내비게이션 | 텍스트 링크 + YouTube 버튼 | 로고 + 다크모드 토글만 유지 (링크 제거) |
| About 페이지 | 배너 이미지만 존재 (내용 없음) | 프로필, 스킬 바, 타임라인 섹션 |
| Blog 페이지 | 배너 이미지만 존재 (내용 없음) | 아티클 카드 리스트, 태그 필터 |
| 반응형 | 미지원 | 모바일 · 태블릿 · 데스크톱 완전 지원 |
| 다크모드 | 없음 | 다크 / 라이트 토글 |
| 애니메이션 | 없음 | 페이지 진입 · 카드 호버 · 스크롤 애니메이션 |

---

## 2. 타깃 사용자

- 채용 담당자 · HR 리크루터
- 협업 제안 개발자 · 기획자
- 개인 작업물에 관심 있는 일반 방문객

---

## 3. 기술 스택

| 레이어 | 기술 |
|--------|------|
| 프레임워크 | Next.js 14 (App Router) + TypeScript |
| 스타일링 | Tailwind CSS v3 + CSS Modules (복잡한 애니메이션) |
| 애니메이션 | Framer Motion |
| 아이콘 | Lucide React |
| 폰트 | Pretendard (한글), Inter (영문) via next/font |
| 배포 | Vercel (정적 내보내기 가능) |
| 컨텐츠 | MDX (블로그 포스트), 로컬 JSON (프로젝트 데이터) |

---

## 4. 디자인 시스템

### 4.1 컬러 팔레트

```
-- 메인 컬러 (딥 퍼플 → 블루 계열, 참고 사이트 분위기 유지)
--color-primary:     #7C3AED   /* violet-600 */
--color-primary-light: #A78BFA /* violet-400 */
--color-accent:      #3B82F6   /* blue-500 */
--color-accent-glow: #60A5FA   /* blue-400 */

-- 배경 (다크 모드 기본)
--color-bg:          #0A0A0F
--color-surface:     #13131A
--color-surface-2:   #1C1C2A
--color-border:      #2D2D3F

-- 텍스트
--color-text-primary:   #F1F0FF
--color-text-secondary: #9D9CB8
--color-text-muted:     #5A5873

-- 라이트 모드 오버라이드 (prefers-color-scheme / 토글)
--color-bg:          #F8F7FF
--color-surface:     #FFFFFF
--color-surface-2:   #F0EFF8
--color-border:      #DDD9F0
--color-text-primary:   #1A1828
--color-text-secondary: #4A4760
```

### 4.2 타이포그래피

| 역할 | 폰트 | 크기 | 굵기 |
|------|------|------|------|
| 사이트명 | Pretendard | 1.125rem | 700 |
| Hero 헤드라인 | Pretendard | 3.5–5rem (clamp) | 800 |
| 섹션 제목 | Pretendard | 1.75rem | 700 |
| 카드 제목 | Pretendard | 1.125rem | 600 |
| 본문 | Pretendard / Inter | 0.9375rem | 400 |
| 코드 | JetBrains Mono | 0.875rem | 400 |

### 4.3 공간감 & 반경

- 기본 그리드: 8px 기준 배수
- 카드 반경: `rounded-2xl` (16px)
- 버튼 반경: `rounded-full` 또는 `rounded-xl`
- 섹션 패딩: 모바일 24px, 데스크톱 80px

### 4.4 이펙트 & 애니메이션 원칙

- 카드 호버: `translateY(-6px)` + 보더 글로우 (`box-shadow: 0 0 20px var(--color-primary)`)
- 페이지 진입: Framer Motion `fadeInUp` (stagger 0.1s)
- 스크롤 트리거: `IntersectionObserver` + Framer Motion `whileInView`
- 배경: 서브틸한 노이즈 텍스처 또는 메쉬 그라디언트 (참고 사이트의 파동 이미지 대체)
- 전환 시간: 200–350ms ease-out (빠르고 군더더기 없이)

---

## 5. 페이지 및 기능 요구사항

### 5.1 공통 — 네비게이션 바 (`<Header>`)

**요구사항**
- 스크롤 시 배경 블러 처리 (`backdrop-filter: blur(16px)` + 반투명)
- 로고 (아이콘 + 사이트명) — 클릭 시 홈으로
- 우측: 다크/라이트 모드 토글 아이콘

**컴포넌트 구조**
```
<Header>
  <Logo />
  <HeaderActions>
    <ThemeToggle />
  </HeaderActions>
</Header>
```

---

### 5.2 홈 페이지 (`/`)

#### 5.2.1 Hero 섹션

```
┌─────────────────────────────────────────────┐
│  [배경: 메쉬 그라디언트 or 파티클 애니메이션]  │
│                                             │
│   안녕하세요, [이름]입니다.                   │
│   Game Developer & AI Engineer              │
│                                             │
│   Unity · Unreal · AI 를 다루는              │
│   개발자의 프로젝트 포트폴리오입니다.           │
│                                             │
│   [프로젝트 보기 →]   [About Me →]           │
│                                             │
│   ──── 스크롤 인디케이터 (아래 화살표) ────    │
└─────────────────────────────────────────────┘
```

**요구사항**
- 전체 뷰포트 높이 (100vh)
- 헤드라인 타이핑 효과 (선택)
- CTA 버튼 2개: "프로젝트 보기" (primary), "About Me" (ghost)
- 배경: CSS 메쉬 그라디언트 (보라 → 파랑) + 미묘한 노이즈 레이어

#### 5.2.2 카테고리 탭 + 프로젝트 갤러리

**카테고리 탭** (사이드바 대신 상단 탭으로 개선)
```
[전체] [Unity] [Unreal] [AI]
```
- 선택된 탭: 보더 언더라인 + 색상 강조
- URL 쿼리 파라미터 동기화 (`?category=unity`)
- 탭 전환 시 카드 페이드+슬라이드 전환 (Framer Motion `AnimatePresence`)

**프로젝트 카드**

```
┌─────────────────────────┐
│  [썸네일 이미지]          │  ← 16:9 비율, object-cover
│  [카테고리 배지]          │  ← 우상단 오버레이 (Unity/Unreal/AI)
├─────────────────────────┤
│  프로젝트 제목            │
│  짧은 설명 (2줄 말줄임)   │
│  날짜                    │
├─────────────────────────┤
│  [GitHub] [영상] [.exe]  │  ← 아이콘 버튼
└─────────────────────────┘
```

**요구사항**
- 그리드: 데스크톱 3열, 태블릿 2열, 모바일 1열
- 호버 시: 카드 올라옴 + 보더 글로우 + 썸네일 약간 확대 (scale 1.03)
- 링크 아이콘: GitHub (Lucide), 영상 (Play), 다운로드 (.exe)
- 데이터 소스: `/data/projects.json`

**projects.json 스키마**
```json
{
  "id": "string",
  "title": "string",
  "description": "string",
  "category": "unity | unreal | ai",
  "date": "YYYY.MM.DD",
  "thumbnail": "/images/projects/xxx.png",
  "links": {
    "github": "url | null",
    "video": "url | null",
    "download": "url | null"
  },
  "tags": ["string"]
}
```

#### 5.2.3 스킬 섹션 (홈 하단)

```
┌──────────────────────────────────────────┐
│  기술 스택                                │
│                                          │
│  Unity   ████████░░  80%                 │
│  Unreal  ██████░░░░  60%                 │
│  Python  ████████░░  80%                 │
│  PyTorch ███████░░░  70%                 │
│  C#      ████████░░  80%                 │
└──────────────────────────────────────────┘
```

- 스크롤 진입 시 바 애니메이션 (0 → n%)
- 좌우 2열 레이아웃 (데스크톱)

---

### 5.3 About 페이지 (`/about`)

#### 섹션 구성

1. **프로필 섹션**
   ```
   [프로필 사진]  이름
                 한 줄 소개
                 이메일 · GitHub · YouTube 아이콘 링크
   ```

2. **소개 텍스트**
   - 3–5문단 자기소개 (Markdown 렌더링)

3. **기술 스택 배지**
   - 카테고리별 (Languages / Game Engine / AI/ML / Tools)
   - 뱃지 형태: 아이콘 + 텍스트, 호버 시 색상 강조

4. **타임라인 (경력 / 학업)**
   ```
   ● 2026.01  AI 강화학습 프로젝트 시리즈 완료
   ● 2025.11  흑길동: 대유쾌 마운틴 출시
   ● 2025.10  WBC Survivor 완성
   ● 2025.06  블렌더 Unreal 연동 프로젝트
   ● 2024.11  Python 게임 첫 프로젝트
   ```
   - 세로 선 + 원형 마커 디자인
   - 스크롤 진입 시 순차 등장

5. **연락처 CTA**
   ```
   [이메일 보내기]  [GitHub 방문]
   ```

---

### 5.4 Blog 페이지 (`/blog`)

#### 섹션 구성

1. **헤더**
   - 제목: "블로그"
   - 부제: "개발 과정, 배운 것들을 기록합니다."

2. **태그 필터**
   ```
   [전체] [Unity] [AI] [개발일지] [회고]
   ```

3. **아티클 카드 리스트**
   ```
   ┌────────────────────────────────────────┐
   │  [썸네일]  제목                         │
   │           날짜 · 읽는 시간 · 태그 뱃지  │
   │           요약 (2줄)                    │
   └────────────────────────────────────────┘
   ```
   - 데스크톱: 2열 그리드
   - 모바일: 1열
   - 최신순 정렬 (기본)

4. **아티클 상세 페이지** (`/blog/[slug]`)
   - MDX 렌더링 (코드 하이라이팅: Shiki)
   - 읽기 진행 바 (상단)
   - 이전/다음 글 네비게이션
   - 목차 (TOC, 우측 고정 — 데스크톱)

---

### 5.5 404 페이지

- 간단한 에러 메시지 + 홈으로 돌아가기 버튼
- 배경: Hero와 동일한 그라디언트

---

## 6. 반응형 브레이크포인트

| 이름 | 범위 | 주요 변화 |
|------|------|-----------|
| mobile | < 640px | 1열 그리드, 햄버거 메뉴, 패딩 축소 |
| tablet | 640–1023px | 2열 그리드, 탭 유지 |
| desktop | ≥ 1024px | 3열 그리드, 사이드 TOC, 풀 네비게이션 |

---

## 7. 성능 요구사항

| 지표 | 목표 |
|------|------|
| Lighthouse Performance | ≥ 90 |
| LCP | < 2.5s |
| CLS | < 0.1 |
| FID / INP | < 100ms |
| 이미지 | next/image WebP 자동 변환, lazy loading |
| 번들 크기 | 초기 JS < 200KB gzipped |

---

## 8. 접근성 (a11y)

- 모든 인터랙티브 요소에 `aria-label`
- 키보드 탐색 완전 지원 (포커스 링 시각화)
- 색상 대비 WCAG AA 기준 이상
- 스크린 리더 시멘틱 HTML (`<nav>`, `<main>`, `<article>`, `<section>`)
- 이미지 `alt` 텍스트 필수

---

## 9. SEO

- `<title>`, `<meta description>` 페이지별 커스터마이징
- Open Graph / Twitter Card 메타태그
- 정적 `sitemap.xml` 자동 생성 (Next.js 기능)
- `robots.txt` 설정

---

## 10. 파일 구조 (예시)

```
/
├── app/
│   ├── layout.tsx          # 루트 레이아웃 (Header, Footer)
│   ├── page.tsx            # 홈 (Hero + 갤러리 + 스킬)
│   ├── about/
│   │   └── page.tsx
│   ├── blog/
│   │   ├── page.tsx        # 블로그 리스트
│   │   └── [slug]/
│   │       └── page.tsx    # 아티클 상세
│   └── not-found.tsx
├── components/
│   ├── layout/
│   │   ├── Header.tsx
│   │   └── Footer.tsx
│   ├── home/
│   │   ├── HeroSection.tsx
│   │   ├── CategoryTabs.tsx
│   │   ├── ProjectGrid.tsx
│   │   ├── ProjectCard.tsx
│   │   └── SkillSection.tsx
│   ├── about/
│   │   ├── ProfileSection.tsx
│   │   ├── SkillBadges.tsx
│   │   └── Timeline.tsx
│   ├── blog/
│   │   ├── ArticleCard.tsx
│   │   ├── TagFilter.tsx
│   │   └── TableOfContents.tsx
│   └── ui/
│       ├── Button.tsx
│       ├── Badge.tsx
│       └── ThemeToggle.tsx
├── data/
│   └── projects.json
├── content/
│   └── blog/
│       └── *.mdx
├── public/
│   └── images/
│       └── projects/
└── styles/
    └── globals.css
```

---

## 11. 스코프 외 (MVP 제외)

- 사용자 인증 / 댓글 기능
- CMS 연동 (Sanity, Contentlayer 등) — MDX 파일 기반으로 충분
- 다국어 (i18n)
- 검색 기능 (블로그 글 수가 적을 때는 불필요)

---

## 12. 마일스톤

| 단계 | 내용 |
|------|------|
| M1 | 디자인 시스템 · 공통 컴포넌트 (Header, Footer, Button, Badge) |
| M2 | 홈 페이지 — Hero + 프로젝트 갤러리 + 카테고리 탭 |
| M3 | About 페이지 — 프로필 + 스킬 + 타임라인 |
| M4 | Blog 리스트 + 상세 페이지 (MDX) |
| M5 | 다크/라이트 모드, 반응형, 애니메이션 마무리 |
| M6 | SEO · 성능 최적화 · Vercel 배포 |
