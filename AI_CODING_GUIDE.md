# AI_CODING_GUIDE.md

## 1. 문서 목적

이 문서는 `musique-mp3/first-class` 저장소의 **현재 버거킹 로그인 UI 코드**를 기준으로,
앞으로 다른 브랜드의 로그인 UI 또는 버거킹의 추가 화면을 만들 때 AI가 기존 코드의 구조와 스타일을 임의로 바꾸지 않고 작업하기 위한 기준이다.

> 핵심 원칙: **새로 잘 만드는 것보다, 현재 작성된 코드의 구조와 작성 방식을 먼저 이해하고 유지한다.**

이 문서는 새로운 화면을 만들 때 참고하는 **AI 코딩 가이드**이며, 현재 코드를 자동으로 리팩터링하거나 수정하기 위한 문서가 아니다.

---

## 2. 기준 코드

현재 기준이 되는 파일은 다음과 같다.

| 파일 | 역할 |
|---|---|
| `burgerking/login.html` | 버거킹 로그인 화면의 HTML + 화면 전용 CSS |
| `css/default.css` | 전역 reset 및 공통 기본 스타일 |
| `font/css/bkbulmatpro _bold.css` | BKR 폰트 정의 |
| `font/css/pretendardvariable.css` | Pretendard Variable 폰트 정의 |
| `font/css/sandollgothicneoround.css` | Sandoll GothicNeoRound 폰트 정의 |
| `burgerking/img/` | 로그인 화면에서 사용하는 SVG 및 이미지 자산 |

현재 `burgerking/login.html`에는 별도의 `login.css` 파일이 없으며,
로그인 화면 전용 스타일은 HTML 내부의 `<style>`에 작성되어 있다.

따라서 AI는 새 화면을 만들 때 **별도의 CSS 파일을 임의로 분리하지 않는다.**
사용자가 별도 분리를 요청한 경우에만 변경한다.

---

# 3. 현재 HTML 구조

현재 버거킹 로그인 화면은 다음 구조를 기준으로 한다.

```html
<body>
    <div id="wrap">

        <header>
            <h1>로그인</h1>

            <button class="prev_bttn">
                <span class="sr-only">이전버튼</span>
            </button>
        </header>

        <main>

            <h2 class="title">
                <span>안녕하세요:)</span>
                <span>버거킹입니다.</span>
            </h2>

            <form action="">
                <fieldset>
                    <legend class="sr-only">로그인 화면</legend>

                    <label for="email" id="email" class="email">
                        이메일 로그인
                    </label>

                    <div class="input_box">
                        <input
                            type="email"
                            id="email"
                            name="email"
                            placeholder="아이디(이메일)를 입력해 주세요"
                        >
                    </div>

                    <div class="input_box rela">
                        <input
                            type="password"
                            name="pw"
                            placeholder="비밀번호를 입력해 주세요"
                        >

                        <button type="button" class="pw_bttn">
                            <span class="sr-only">비밀번호 보기</span>
                        </button>
                    </div>

                    <div class="login_option">
                        <label>
                            <input
                                type="checkbox"
                                class="check sr-only"
                                checked
                            >
                            <span>자동 로그인</span>
                        </label>

                        <label>
                            <input
                                type="checkbox"
                                class="check sr-only"
                            >
                            <span>아이디 저장</span>
                        </label>
                    </div>

                    <button type="submit" class="login_bttn">
                        로그인
                    </button>

                </fieldset>
            </form>

            <div class="login_link">
                <a href="#">아이디 찾기</a>
                <a href="#">비밀번호 재설정</a>
                <a href="#">회원가입</a>
            </div>

            <div class="sns_login">
                <p>SNS로 간편하게 로그인</p>

                <div class="sns_list">
                    <a href="#">
                        <span class="sr-only">카카오로그인</span>
                    </a>
                    <a href="#">
                        <span class="sr-only">네이버로그인</span>
                    </a>
                    <a href="#">
                        <span class="sr-only">애플로그인</span>
                    </a>
                    <a href="#">
                        <span class="sr-only">삼성카드로그인</span>
                    </a>
                </div>
            </div>

        </main>

    </div>
</body>
```

## 3-1. 구조를 유지해야 하는 이유

새 화면을 만들 때 위 구조를 그대로 복사하라는 뜻은 아니다.

대신 다음과 같은 **구조적 특징**을 유지한다.

- `body > #wrap > header + main` 구조
- 화면 상단 제목은 `header > h1`
- 실제 화면 내용은 `main`
- 사용자 입력이 있는 화면은 `form`을 사용
- 관련 입력은 `fieldset`으로 묶을 수 있다
- 화면의 주요 콘텐츠는 `h1`, `h2` 등 기존 계층을 참고한다
- 기존 클래스명 작명 방식과 계층 구조를 최대한 유지한다
- 버튼, 링크, 입력창 등 HTML 요소의 의미는 유지한다
- 접근성용 `.sr-only`를 기존 방식대로 사용한다

새 화면의 콘텐츠가 달라지면 HTML 구조도 달라질 수 있다.
**버거킹 로그인 화면에 없는 요소를 억지로 추가하지 않는다.**

---

# 4. 현재 CSS 구조

## 4-1. 전역 CSS

`css/default.css`는 다음 역할을 담당한다.

- box-sizing
- margin / padding 초기화
- body 기본 설정
- 한국어 줄바꿈
- 리스트 초기화
- 링크 기본 스타일
- 이미지 / SVG 기본 스타일
- form 요소 기본 스타일
- button 기본 스타일
- focus-visible
- `.sr-only`
- reduced motion
- hidden
- touch 관련 기본 설정
- fieldset / legend 초기화

따라서 새 화면에서 이미 `default.css`가 제공하는 기본값을 다시 작성하지 않는다.

예:

```css
*,
*::before,
*::after {
    box-sizing: border-box;
}
```

와 같은 reset을 화면별 CSS에 다시 만들지 않는다.

---

## 4-2. 화면 전용 CSS

현재 버거킹 로그인 화면의 전용 CSS는 `burgerking/login.html` 내부 `<style>`에 있다.

CSS는 다음 순서와 역할을 가진다.

1. CSS 변수
2. html / body
3. `#wrap`
4. header
5. 제목
6. input
7. 비밀번호 보기 버튼
8. 로그인 옵션
9. 로그인 버튼
10. 링크
11. SNS 로그인
12. 이전 버튼
13. 체크박스

새 화면을 만들 때도 가능한 한 **큰 구조 → 콘텐츠 → 상태/컨트롤 → 세부 요소** 순서로 작성한다.

---

# 5. 현재 CSS 변수

버거킹 로그인 화면은 다음 변수를 사용한다.

```css
:ROOT {
    --font-neo: "Sandoll GothicNeoRound", sans-serif;
    --font-pre: "Pretendard Variable", sans-serif;
    --font-bkr: "BKR", sans-serif;

    --primary: #512314;
    --focus: #d62302;
    --baseborder: #d5cdc2;
    --inputbg: #fffcf9;
    --button: #e9ddcd;
    --errorcolor: #c54734;
    --placeholder: #ebe6e2;
    --text: #766053;
    --background: #f4ebdc;
}
```

## AI 작업 규칙

다른 브랜드 UI를 만들 때는 **버거킹의 색상값을 그대로 가져가는 것이 목적이 아니다.**

다음은 유지한다.

- CSS 변수로 디자인 값을 관리하는 방식
- 변수 이름을 의미 중심으로 작성하는 방식
- 폰트도 변수로 관리하는 방식

브랜드가 달라지면 색상값은 해당 브랜드의 디자인에 맞게 변경할 수 있다.

예:

```css
:root {
    --primary: 브랜드 주요 색상;
    --background: 브랜드 배경 색상;
}
```

단, AI가 임의로 브랜드 컬러를 결정하지 않는다.
사용자가 제공한 디자인 또는 조사된 브랜드 기준을 우선한다.

---

# 6. 폰트 사용 규칙

현재 저장소에는 다음 폰트가 정의되어 있다.

### BKR

```css
font-family: "BKR", sans-serif;
```

버거킹의 브랜드용 폰트로 사용한다.

현재 로그인 화면에서는 `.title`에 적용되어 있다.

### Pretendard Variable

```css
font-family: "Pretendard Variable", sans-serif;
```

현재 로그인 화면에서는 링크 영역 등에 사용한다.

### Sandoll GothicNeoRound

```css
font-family: "Sandoll GothicNeoRound", sans-serif;
```

현재 body 기본 폰트로 사용한다.

## AI 규칙

- 기존 폰트 파일을 임의로 삭제하지 않는다.
- 다른 브랜드 화면에서 버거킹 전용 BKR 폰트를 무조건 사용하지 않는다.
- 새 브랜드의 폰트가 별도로 제공되면 해당 폰트를 추가하고 CSS 변수 방식은 유지한다.
- 폰트가 없는 경우 임의로 외부 폰트를 추가하지 않는다.
- 폰트 변경은 브랜드 디자인 또는 사용자의 지시에 근거한다.

---

# 7. 화면 크기와 레이아웃 기준

현재 `#wrap`은 다음 기준을 사용한다.

```css
#wrap {
    width: 100%;
    max-width: 1024px;
    min-width: 360px;
    margin: 0 auto;
    background-color: var(--background);
    min-height: 100dvh;
}
```

따라서 새 화면도 기본적으로 다음 원칙을 따른다.

- width: 100%
- max-width: 1024px
- min-width: 360px
- 가운데 정렬
- 최소 화면 높이 확보
- `100dvh` 사용
- 모바일 화면을 우선 고려

현재 main은 다음과 같다.

```css
main {
    padding: 48px 20px 90px;
}
```

새 화면에서 다른 여백이 필요하다면 디자인을 기준으로 변경하되,
단순히 개인 취향으로 전체 레이아웃 방식을 바꾸지 않는다.

---

# 8. rem 사용 규칙

현재 로그인 화면은 다음 기준을 사용한다.

```css
html {
    font-size: 62.5%;
}
```

따라서 주요 글자 크기는 `rem`으로 작성한다.

예:

```css
body {
    font-size: 1.6rem;
}

h1 {
    font-size: 2rem;
}
```

새 화면에서도 기존 화면과 일관성이 필요한 경우 이 방식을 유지한다.

---

# 9. 입력창 스타일

현재 이메일 / 비밀번호 입력창은 공통 스타일을 사용한다.

```css
input[type="email"],
input[type="password"] {
    width: 100%;
    height: 50px;
    padding: 0 20px;
    border-radius: 10px;
}

input {
    border: 1px solid var(--baseborder);
    background-color: var(--inputbg);
}

input::placeholder {
    color: var(--placeholder);
}
```

새로운 로그인/회원 관련 화면을 만들 때 입력창의 기본 스타일은 이 기준을 먼저 사용한다.

디자인에서 차이가 확인되는 경우에만 변경한다.

---

# 10. 버튼 스타일

현재 로그인 버튼은 다음 특징을 가진다.

- width: 100%
- height: 44px
- border-radius: 25px
- `var(--primary)` 사용
- 비활성 상태처럼 보이도록 opacity 사용
- font은 inherit

새 화면에서도 버튼 스타일을 새로 발명하기보다
현재 버튼의 구조를 먼저 참고한다.

다만 버튼의 상태가 실제로 활성/비활성인지 확인하고,
단순히 시각적으로 비슷하다는 이유로 `disabled` 속성을 임의로 추가하지 않는다.

---

# 11. 아이콘과 이미지 경로

현재 버거킹 로그인 UI의 자산은 주로 다음 폴더에 있다.

```
burgerking/img/
```

주요 파일 예:

- `Back.svg`
- `Eye.svg`
- `Kakao.svg`
- `Naver.svg`
- `Apple.svg`
- `Samsung.svg`
- `Property 1=Active.svg`
- `Property 1=Disabled.svg`

현재 CSS에서는 SVG를 background-image로 사용하는 방식이 있다.

예:

```css
.pw_bttn {
    background: url(../burgerking/img/Eye.svg)
        no-repeat center / 20px;
}
```

새 화면에서 기존 자산을 사용할 경우 **실제 파일이 존재하는지 먼저 확인한다.**

새 아이콘이 필요한 경우:

1. 저장소에 기존 자산이 있는지 확인
2. 기존 자산으로 대체할 수 있는지 확인
3. 사용자가 제공한 자산이 있는지 확인
4. 그래도 없으면 사용자에게 필요한 자산을 알려준다

AI가 임의로 SVG 코드를 새로 만들거나 외부 CDN을 추가하지 않는다.

---

# 12. 상대 경로 확인

현재 `burgerking/login.html`은 `burgerking` 폴더 안에 있기 때문에
파일 경로는 **현재 HTML 파일의 위치를 기준으로 판단해야 한다.**

예:

```
burgerking/login.html
burgerking/img/Back.svg
```

이면 다음 경로는 정상이다.

```html
img/Back.svg
```

또는 현재 코드처럼

```css
url(../burgerking/img/Eye.svg)
```

와 같이 작성된 경로도 실제 파일 위치를 기준으로 동작할 수 있다.

새 화면을 만들 때는 경로를 임의로 정리하지 말고,
**실제 폴더 구조를 먼저 확인한 뒤 작성한다.**

---

# 13. 접근성 코드

현재 전역 CSS에는 다음 `.sr-only`가 정의되어 있다.

```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

현재 버거킹 UI에서도 아이콘 버튼의 설명에 사용한다.

예:

```html
<button class="pw_bttn">
    <span class="sr-only">비밀번호 보기</span>
</button>
```

새 화면에서도 시각적으로 아이콘만 표시되는 버튼에는
사용자가 그 버튼의 목적을 알 수 있도록 기존 `.sr-only` 방식을 우선 사용한다.

---

# 14. HTML 작성 원칙

새 화면을 만들 때 다음 순서로 판단한다.

### 1단계. 화면의 의미 파악

먼저 이 화면이 무엇인지 판단한다.

예:

- 로그인
- 비밀번호 재설정
- 회원가입
- 아이디 찾기
- 약관 동의

### 2단계. 기존 구조와 비교

버거킹 로그인 화면에서 재사용 가능한 구조를 찾는다.

예:

- header
- h1
- main
- form
- fieldset
- input
- button
- 링크 영역

### 3단계. 달라지는 구조만 추가

새 화면에 필요한 요소만 추가한다.

예를 들어 비밀번호 재설정 화면이라면
비밀번호 입력, 비밀번호 확인, 비밀번호 조건 등이 필요할 수 있다.

### 4단계. 의미에 맞는 HTML 선택

태그는 시각적 모양보다 콘텐츠 의미를 기준으로 선택한다.

- 페이지 제목 → `h1`
- 주요 내용 → `main`
- 관련 콘텐츠 그룹 → `section`
- 사용자 입력 → `form`
- 관련 form 입력 그룹 → `fieldset`
- 반복되는 동일 종류의 항목 → 필요하면 `ul > li`
- 동작 → `button`
- 다른 페이지/주소로 이동 → `a`

단, 기존 프로젝트에서 사용자가 이미 작성한 구조가 있다면
AI가 이유 설명 없이 전체 구조를 다른 방식으로 바꾸지 않는다.

---

# 15. 기존 코드 수정 규칙

AI는 새 화면을 만들 때 다음 행동을 하지 않는다.

### 하지 말 것

- 기존 `login.html`을 전체 교체
- 사용자가 작성한 클래스명을 임의로 변경
- CSS 구조를 다른 방식으로 전면 리팩터링
- 별도의 CSS 파일로 강제 분리
- 새로운 라이브러리 추가
- Bootstrap, Tailwind 등 외부 CSS 프레임워크 추가
- 아이콘 라이브러리 추가
- 외부 CDN 의존성 추가
- 기존 이미지 파일을 다른 이미지로 교체
- 사용자가 요청하지 않은 JavaScript 기능 추가
- 디자인을 임의로 개선한다는 이유로 구조 변경

### 우선할 것

- 현재 코드에서 재사용할 수 있는 구조 찾기
- 변경해야 하는 부분만 수정
- 변경 이유를 설명
- 필요한 파일만 추가
- 기존 경로와 폴더 구조 유지

---

# 16. 다른 브랜드 로그인 UI를 만들 때의 작업 순서

다른 브랜드의 로그인 UI를 만들 경우 다음 순서를 따른다.

## STEP 1. 기존 버거킹 코드 확인

먼저 다음 파일을 읽는다.

```
burgerking/login.html
css/default.css
font/css/*
```

## STEP 2. 새로운 디자인 확인

Figma, 이미지 또는 사용자가 제공한 디자인을 확인한다.

확인할 내용:

- 화면 크기
- header
- 제목
- 입력 영역
- 버튼
- 링크
- SNS 로그인
- 아이콘
- 색상
- 폰트
- 간격
- 상태값

## STEP 3. 공통점과 차이점 분리

예:

| 구분 | 버거킹 | 새 브랜드 |
|---|---|---|
| 전체 구조 | header + main | 유지 가능 |
| 폼 | form + fieldset | 유지 가능 |
| 입력창 | 50px | 디자인에 따라 변경 |
| 버튼 | 둥근 버튼 | 브랜드 디자인에 따라 변경 |
| 색상 | 버거킹 컬러 | 새 브랜드 컬러 |
| 폰트 | BKR / Sandoll / Pretendard | 새 브랜드 기준 |
| 아이콘 | 버거킹 SVG | 새 브랜드 자산 |
| SNS | 4개 | 디자인에 따라 변경 |

## STEP 4. 폴더와 파일 확인

새 브랜드 폴더를 만들기 전에 실제 저장소 구조를 확인한다.

예:

```
brand-name/
├─ login.html
└─ img/
```

단, 실제 구조는 프로젝트 진행 상황에 따라 결정한다.

## STEP 5. HTML 먼저 작성

기존 버거킹의 구조를 참고하여
새 브랜드 화면의 콘텐츠와 의미에 맞는 HTML을 만든다.

## STEP 6. CSS 적용

기존 CSS 작성 방식을 참고하여
색상, 폰트, 크기, 여백, 아이콘 등을 변경한다.

## STEP 7. 경로 확인

HTML과 CSS에서 사용하는 모든 이미지/폰트 경로를 확인한다.

## STEP 8. 최종 비교

다음 항목을 확인한다.

- 기존 구조와 일관적인가?
- 새 브랜드 디자인과 일치하는가?
- 모바일에서 깨지지 않는가?
- 이미지 경로가 정상인가?
- 폰트가 정상적으로 로드되는가?
- 버튼과 input의 의미가 맞는가?
- 불필요한 코드가 추가되지 않았는가?

---

# 17. JavaScript 규칙

현재 버거킹 로그인 화면은 HTML/CSS 중심이며,
이 가이드는 JavaScript를 기본적으로 추가하지 않는다.

새 화면에서 다음과 같은 기능이 실제로 필요할 때만 JavaScript를 고려한다.

- 비밀번호 보이기/숨기기
- 체크박스 상태 변화
- 입력값 검증
- 버튼 활성화
- 간단한 UI 상태 변경

JavaScript를 추가할 때도 외부 라이브러리를 먼저 사용하지 않는다.

가능하면 현재 학습 범위의 기본 JavaScript로 구현한다.

---

# 18. AI의 답변 방식

AI가 새 화면 코드를 만들기 전에는 다음 내용을 먼저 간단히 설명한다.

### ① 재사용하는 부분

예:

- `#wrap`
- `header`
- `main`
- form 구조
- input 기본 스타일
- `.sr-only`

### ② 변경하는 부분

예:

- 브랜드 색상
- 폰트
- 제목
- 아이콘
- SNS 종류
- 버튼 디자인

### ③ 새로 추가하는 부분

예:

- 비밀번호 조건
- 추가 입력창
- 약관 영역

그 다음 필요한 코드를 작성한다.

---

# 19. 오류가 발생했을 때

오류가 생기면 바로 전체 코드를 다시 작성하지 않는다.

먼저 다음 순서로 확인한다.

1. HTML 구조
2. CSS 선택자
3. 클래스명
4. 파일 경로
5. 이미지 파일 존재 여부
6. 폰트 경로
7. 저장 여부
8. Live Server / 브라우저 상태
9. 그 다음 JavaScript

특히 이미지가 보이지 않는 경우에는
CSS 문제라고 바로 판단하지 말고 **파일 경로와 실제 파일 위치부터 확인한다.**

---

# 20. 현재 코드의 특이사항

현재 기준 코드를 분석하면 다음과 같은 부분이 존재한다.

### 20-1. `id="email"` 중복

현재 코드에는 다음과 같이 `id="email"`이 두 번 사용되어 있다.

```html
<label for="email" id="email" class="email">
<input type="email" id="email" ...>
```

HTML에서 `id`는 문서 안에서 고유해야 한다.

그러나 이 가이드에서는 사용자가 요청하지 않은 기존 코드 전체 수정은 하지 않는다.

새 화면을 작성할 때는 **새로 작성하는 요소의 id를 중복시키지 않는다.**

### 20-2. 현재 SNS 영역은 `a` 사용

현재 SNS 로그인 항목은 다음 구조다.

```html
<a href="#">
    <span class="sr-only">카카오로그인</span>
</a>
```

새 화면에서도 기존 프로젝트의 작성 방식을 우선 참고한다.

다만 실제 기능이 동작하는 방식에 따라 `a`와 `button` 중 어떤 것이 의미에 맞는지 판단할 수 있으며,
변경할 경우 그 이유를 먼저 설명한다.

### 20-3. 현재 SNS 영역은 `div`

현재 SNS 로그인 영역은 `div.sns_login`과 `div.sns_list`로 구성되어 있다.

AI가 새 화면에서 이 구조를 자동으로 `section`이나 `ul`로 변경하지 않는다.

**기존 코드의 일관성을 유지하는 것이 이 프로젝트의 우선 목표이기 때문이다.**

### 20-4. 현재 로그인 버튼의 opacity

현재 로그인 버튼에는 다음 스타일이 있다.

```css
opacity: 0.2;
```

이것은 화면에서 비활성처럼 보이는 상태를 표현하는 현재 디자인 코드다.

실제 HTML의 `disabled` 상태와 동일하다고 단정하지 않는다.

---

# 21. 가장 중요한 원칙

새로운 브랜드 UI를 만들 때 다음 우선순위를 따른다.

```
1. 사용자가 제공한 디자인
        ↓
2. 현재 버거킹 UI 코드
        ↓
3. 현재 프로젝트의 폴더 / 파일 구조
        ↓
4. HTML / CSS 기본 원칙
        ↓
5. 필요한 경우에만 새로운 방법 제안
```

AI는 기존 코드를 보고
**"더 좋은 코드"라는 이유만으로 프로젝트의 작성 방식을 바꾸지 않는다.**

이 프로젝트의 목적은 완성된 코드를 받는 것이 아니라,
현재 작성한 코드를 기반으로 다른 화면을 직접 만들어보면서
HTML / CSS 구조와 디자인 시스템을 이해하는 것이다.

---

## 22. 한 줄 요약

> **버거킹 로그인 UI를 복제하는 것이 아니라, 버거킹 코드의 구조와 작성 방식을 기준으로 새로운 브랜드의 디자인만 교체하고 필요한 부분만 확장한다.**
