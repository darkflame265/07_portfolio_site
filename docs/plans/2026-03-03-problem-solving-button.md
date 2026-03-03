# "문제 해결" 버튼 추가 Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 로비(index.html) hero-cta 영역의 GitHub 버튼 오른쪽에 portfolio.html로 이동하는 "문제 해결" accent 버튼 추가

**Architecture:** index.html 단일 파일 수정. CSS에 `.btn-accent` 스타일 추가, hero-cta div에 링크 요소 추가.

**Tech Stack:** Vanilla HTML/CSS (no build step)

---

### Task 1: `.btn-accent` CSS 추가

**Files:**
- Modify: `index.html` (`.btn-ghost` 스타일 블록 바로 아래)

**Step 1: CSS 삽입**

`.btn-ghost:hover { ... }` 블록 바로 뒤에 아래 CSS를 추가한다:

```css
.btn-accent {
  background: linear-gradient(135deg, var(--accent), #0891b2 80%);
  color: #fff;
  box-shadow: 0 0 28px var(--accent-glow);
}
.btn-accent::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, #22d3ee, var(--accent));
  border-radius: inherit;
  opacity: 0;
  transition: opacity 0.2s;
}
.btn-accent:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 36px var(--accent-glow);
}
.btn-accent:hover::before {
  opacity: 1;
}
```

**Step 2: 확인**

`index.html`을 열어 CSS 추가 위치가 올바른지 확인 (`.btn-ghost:hover` 블록 직후).

---

### Task 2: "문제 해결" 버튼 HTML 추가

**Files:**
- Modify: `index.html` (hero-cta div, GitHub `</a>` 태그 바로 뒤)

**Step 1: 버튼 HTML 삽입**

GitHub `</a>` 닫는 태그 직후, `</div>` (hero-cta 닫기) 직전에 아래를 추가한다:

```html
          <a href="portfolio.html" class="btn btn-accent">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="10"/>
              <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/>
              <line x1="12" y1="17" x2="12.01" y2="17"/>
            </svg>
            문제 해결
          </a>
```

**Step 2: 브라우저에서 확인**

`index.html`을 브라우저로 열어 3개 버튼이 나란히 표시되고, 버튼 클릭 시 `portfolio.html`로 이동하는지 확인.

---

### Task 3: 커밋

```bash
git add index.html
git commit -m "feat: 로비에 문제 해결 버튼 추가 (portfolio.html 링크)"
```
