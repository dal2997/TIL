# 🧯 Tailwind `dark:` 라이트에서 글씨가 하얘지는 문제 — 최종 원인 & 해결 기록

## 날짜
2026-02-10

## 증상
- 라이트 모드인데 Hero 영역(`#hero h1`) 텍스트가 안 보임
- `getComputedStyle(...).color` → `rgb(255, 255, 255)`
- DOM 상 `html`에 `.dark` 클래스 없음
- Tailwind `darkMode: "class"` 이미 설정되어 있음
- 코드상 `text-white` 하드코딩 없음 (rg로 전수 검색 확인)

## 핵심 진단
```js
[
  document.documentElement.className,                // "light"
  document.documentElement.classList.contains("dark"), // false
  window.matchMedia("(prefers-color-scheme: dark)").matches // true
]
// 결과: ['light', false, true]
```

👉 **OS / 브라우저의 다크 선호(prefers-color-scheme: dark)가 true**
👉 Tailwind의 `dark:` variant가 **미디어 쿼리 기반으로 작동**
👉 `.dark` 클래스가 없어도 `dark:text-white`가 적용됨
👉 라이트 화면인데 글자가 흰색으로 덮임

즉,
> ❌ Hero.tsx 문제 아님  
> ❌ class 중복 문제 아님  
> ❌ ThemeToggle / ThemeProvider 문제 아님  
> ✅ Tailwind v4 + media 기반 dark variant가 원인

---

## 결정적 해결책 (정답)

### `src/app/globals.css` 맨 위에 추가

```css
@import "tailwindcss";

/* ✅ dark: variant를 미디어가 아닌 .dark 클래스 기준으로 강제 */
@custom-variant dark (&:where(.dark, .dark *));
```

### 의미
- `dark:`는 **오직 `.dark` 클래스가 있을 때만** 적용
- OS 다크 선호(`prefers-color-scheme`) **완전히 무시**
- `next-themes`의 `attribute="class"` 전략과 100% 일치

---

## 적용 후 확인 결과
```js
getComputedStyle(document.querySelector("#hero h1")).color
```

- 라이트: `lab(8.30 0.61 -2.16)` (zinc 계열) ✅
- 다크: 정상적으로 white 계열 ✅
- 더 이상 `rgb(255,255,255)` 라이트에서 안 나옴

---

## 왜 이게 며칠을 잡아먹었나
- Tailwind v4에서는 `@import "tailwindcss"` 사용 시  
  **darkMode 설정이 media 기반으로 동작할 수 있음**
- `.dark` 클래스 여부만 보고 “dark 안 걸렸다”고 판단하면 헛다리
- computed style은 맞았고, 원인은 **Tailwind variant 해석 레벨**이었음

---

## 재발 방지 체크리스트
- Tailwind v4 + next-themes 조합이면 무조건 아래 추가
  ```css
  @custom-variant dark (&:where(.dark, .dark *));
  ```
- “라이트인데 white 나오는 문제” →  
  **컴포넌트 말고 variant 해석부터 의심**
- `.closest('.dark') === null` + `prefers-color-scheme: dark === true`
  → 99% 이 케이스

---

## 한 줄 결론
> **문제는 Hero도, class도, ThemeToggle도 아니었고  
Tailwind의 `dark:`가 media 기준으로 살아 있었던 게 원인이었다.**

