# SMACSS

SMACSS(스맥스)는 Scalable and Modular Architecture for CSS(CSS를 위한 확장 가능한 모듈 아키텍처)의 약어다. CSS 코드를 그 역할에 따라 분류한 것이 특징으로 다음 다섯 가지 분류에 따라 각각 규칙을 설정하고 있다.

1. 베이스(Base)
2. 레이아웃(Layout)
3. 모듈(Module)
4. 스테이트(State)
5. 테마(Thema)

OOCSS에서 말한 범위는 SMACSS의 '모듈'과 비슷하다. OOCSS가 거의 모듈만 언급했던 것에 비해 SMACSS는 보다 폭넓고 실제로 웹사이트를 구축하는 데 있어 빼놓을 수 없는 베이스나 레이아웃 코드를 다루는 방법까지 설명한다.

## 베이스 규칙

`1. 특성에 따라 CSS를 분류한다.`

베이스 규칙에서는 프로젝트의 표준 스타일을 정의한다. 베이스 규칙에서 사용하는 셀렉터는 주로 요소형 셀렉터(body {})이지만 그 외에 다음 셀렉터도 사용한다.

-   자녀 셀렉터(a > img {} 등)
-   손자 셀렉터(ul li {} 등)
-   의사 클래스(a:hover {} 등)
-   속성 셀렉터(a[href] {} 등)
-   인접 형제 셀렉터(h2 + p {} 등)
-   일반 형제 셀렉터(h2 - p {} 등)

그러나 기본 규칙을 너무 많이 정의해 놓으면 영향 범위가 넓어져 CSS 설계 포인트 중 '3. 영향 범위를 지나치게 넓히지 않는다'를 위반하게 되므로 주의한다. 또한 '프로젝트 내에서 각 요소가 표준으로 어떻게 동작하는가'를 정의하기 위한 규칙이기 때문에, 특정한 상황에서의 사용을 가정하는 ID 셀렉터나 클래스 셀렉터에는 사용할 수 없다. 같은 이유로 기본 규칙에는 !important도 사용하지 않는다.

코드는 대체로 다음과 같다.

```css
body {
    background-color: #fff;
}

/* 자녀 셀렉터 예시 */
a > img {
    translation: 0.25s;
}

/* 손자 셀렉터 예시 */
ul li {
    margin-bottom: 10px;
}

/* 의사 클래스 예시 */
a:hover {
    text-decoration: underline;
}
```

SMACSS에서는 리셋 CSS 베이스 규칙으로 포함한다. 2장에서 소개한 웹에 공개되어 있는 기존의 리셋 CSS를 사용하는 경우에는 그만큼 코드가 길어질 것이다. 또한 규칙이라 할 수는 없지만 SMACSS에서는 body의 배경 색상을 기본 규칙으로 설정하는 것을 강하게 권장한다. 이는 웹사이트를 보는 사용자가 브라우저 기능을 사용해 배경색을 독자적으로 지정한 경우, 색상에 따라 웹사이트가 정상적으로 보이지 않을 가능성이 있기 때문이다.

## 레이아웃 규칙

`1. 특성에 따라 CSS를 분류한다.`

레이아웃 규칙은 헤더나 메인 영역, 사이드 바, 푸터 등 웹사이트의 큰 틀을 구성하는 큰 모듈에 관한 규칙이다. 레이아웃을 구성하는 것이 대부분은 특정 페이지에서 한 차례만 사용되는 것이 많으므로, ID 셀렉터를 활용한 스타일링을 허용한다. 레이아웃 관련해서 반복적으로 사용하는 모듈의 경우에는 클래스 셀렉터를 이용한다.

다음은 레이아웃 규칙 예시이다.

```css
/* ID 셀렉터 예시 */
#header {
    width: 1080px;
    margin-right: auto;
    margin-left: auto;
    background-color: #fff;
}

#main {
    width: 1080px;
    margin-right: auto;
    margin-left: auto;
    background-color: #fff;
}

#footer {
    width: 1080px;
    margin-right: auto;
    margin-left: auto;
    background-color; #eee
}

/* 클래스 셀렉터 예시 */
.section {
    padding-top: 80px;
    padding-bottom: 80px;
}
```

## 특정한 상황에서 레이아웃이 변경되는 경우

예를 들어, '특정한 페이지에서는 가로 폭을 좁히고 싶은'경우에는 손자 셀렉터를 이용해 레이아웃 모듈의 스타일을 덮어쓸 수 있다. 다음 코드에서는 body 요소에 .l-narrow 클래스를 붙여서 손자 셀렉터를 사용해 헤더, 메인 영역, 푸터의 가로 폭을 좁혔다.

```css
.l-narrow #header {
    width: 960px;
}

.l-narrow #main {
    width: 960px;
}

.l-narrow #footer {
    width: 960px;
}
```

이 코드에서 주목할 부분은 .l-narrow에서 접두사 'l-'이 붙어있다는 점이다. 레이아웃 규칙에서 클래스 셀렉터를 사용할 때는 'l-'이라는 접두사를 붙이는 것을 권장한다.

이는 뒤에서 설명할 모듈 규칙 혹은 스테이트(State) 규칙에 해당하는 모듈과 쉽게 구분할 수 있게 하기 위한 조치이다. 그런 관점에서 보면 .section이라는 클래스 이름 역시 SMACSS에서는 .l-section이라는 이름으로 바꾸는 것이 바람직하다.

주의할 것은 이 접두사는 클래스 셀렉터 뿐만 아니라 ID 셀렉터뿐만 아니라 ID 셀렉터에서도 사용할 수 있다는 것이다. 하지만 ID 셀렉터는 레이아웃 규칙에서만 사용되며 한 페이지 안에서의 사용 빈도도 그리 높지 않으므로 접두사를 붙이지 않아도 충분히 구분할 수 있을 것이다.

ID 셀렉터를 클래스 셀렉터로 변환한 경우의 CSS 예시이다.

```css
.l-header {
    width: 1080px;
    margin-right: auto;
    margin-left: auto;
    background-color: #fff;
}

.l-main {
    width: 1080px;
    margin-right: auto;
    margin-left: auto;
    background-color: #fff;
}

.l-footer {
    width: 1080px;
    margin-right: auto;
    margin-left: auto;
    background-color: #eee;
}

.l-section {
    padding-top: 80px;
    padding-bottom: 80px;
}

/* 가로 폭이 좁아지는 경우 */
.l-narrow .l-header {
    width: 960px;
}
.l-narrow .l-main {
    width: 960px;
}
.l-narrow .l-footer {
    width: 960px;
}
```

## 모듈 규칙

모듈 규칙에 해당하는 모듈이란, 앞에서 설명한 레이아웃 모듈 안에 배치되는 것을 가정하고 있다.

-   타이틀(Title)
-   버튼(Button)
-   카드(Card)
-   네비게이션(Navigation)
-   캐러셀(Carousel)

위와 같이 레이아웃 모듈 안에 배치할 수 있는 개별 모듈이라면 SMACSS에서는 모두 모듈 규칙에 해당한다.

모듈은 다른 페이지로 이동하거나 다른 레이아웃 안에 삽입하더라도 형태가 부서지거나 달라지지 않고 사용할 수 있어야 한다. CSS 설계의 허리가 되는 부분이기도 하므로 구현 시에는

-   '불필요한 코드는 없는가',
-   '이 코드는 다른 레이아웃으로 이동했을 때 영향이 없는가'

등 한층 주의가 필요하다. 한 페이지 내에서 반복해서 사용되는 상황을 가정하고 있으므로 당연히 ID 셀렉터에서의 구현은 하지 않으며, 모듈의 루트 요소에는 반드시 클래스 셀렉터(HTML에서는 클래스 속성)을 사용한다.

모듈을 만들 때는 다음과 같은 두 가지 사항에 주의해야 한다.

-   가급적 요소형 셀렉터를 사용하지 않는다.
-   특정한 콘텍스트에 지나치게 의존하지 않는다.

## 가능한 요소형 셀렉터를 사용하지 않는다.

`2. HTML과 스타일링을 느슨하게 결합한다.`

2장 CSS 설계의 여덟 가지 포인트 중 '2. HTML과 스타일링을 느슨하게 결합한다.'에 설명한 것과 같은 이야기다.

```css
.media {
    display: flex;
    align-items: center;
    font-size: 16px
    line-height: 1.5;
}

.media figure {
    flex: 1 1 25%;
    margin-right: 3.33333%;
}

.media img {
    width: 100%
    vertical-align: top;
}

.media div {
    flex: 1 1 68.33333%;
}

.media p:first-of-type {/* 사용자를 고려한 설계로 만족스러운 체험 */
    margin-bottom: 10px;
    font-size: 18px;
    font-weight: bold;
}

.media p:last-of-type { /* 퍼소나란? */
    margin-top: 10px;
    margin-bottom: 10px;
    font-size: 16px;
    border-bottom: 1px solid #555
}

.media {
    color: #555;
    font-size: 14px;
}
```

이 상태에서는 p 요소의 순서가 바뀌면 제목 부분 스타일이 설명문의 p 요소에 해당하게 되거나, div 요소나 span 요소가 늘어나는 경우 의도치 않게 기존 스타일이 적용되어 버린다.

SMACSS에서는 다음과 같은 해결책을 제시한다.

-   요소를 시멘틱으로 한다.(시멘틱한 요소에만 요소형 셀렉터를 사용한다.)
-   요소형 셀렉터를 사용할 때는 셀렉터를 사용한다.

### 요소를 시멘틱으로 한다.

예를 들면, '사용자를 고려한 설계로 만족스러운 체험을'부분과 '퍼소나란?' 부분은 제목 요소를 적용할 수 있을 법하다. 그것만으로 순서에 관계없이 제목에는 제목용 스타일을 적용할 수 있다. 다만 경우에 따라서는 시멘틱 요소로 치환할 수 없는 경우도 있을 것이다. 특히 div, span 같은 요소에는 시맨틱 특성이 없으므로 SMACSS에서는 이 두 요소에는 반드시 클래스를 붙여서 스타일링을 하는 것을 권장한다.

-   요소를 시맨틱으로 한다.
-   div 요소와 span 요소에는 클래스를 붙인다.

```css
.media {
    display: flex;
    align-items: center;
    font-size: 16px
    line-height: 1.5;
}
.media figure {
    flex: 1 1 25%;
    margin-right: 3.33333%
}
.media figure {
    width: 100%;
    vertical-align: top;
}
.media-body {
    flex: 1 1 68.33333%;
}
.media h2 {
    margin-bottom: 10px;
    font-size: 18px;
    font-weight: bold;
}
.media h3 {
    margin-top: 10px;
    margin-bottom: 18px;
    font-size: 16px;
    border-bottom: 1px solid #555;
}
.media-sub-text {
    color: #555;
    font-size: 14px;
}
```

SMACSS에서 생각할 수 있는 시멘틱 특성에는 다음 공식이 성립한다.

div 요소 / span 요소 등의 범용적인 요소 < 제목 등의 의미를 가진 요소 < 클래스 속성이 붙은 요소

### 요소형 셀렉터를 사용할 때는 자녀 셀렉터를 사용한다.

`3. 영향 범위를 지나치게 넓히지 않는다.`

다른 해결책으로 요소형 셀렉터를 사용할 때는 손자 셀렉터가 아닌 자녀 셀렉터를 사용하는 것을 권한다. 스타일링의 적용 범위를 바로 아래 요소로 한정함으로써 영향 범위를 필요 이상으로 넓히지 않는다.

이를 구현한 코드를 살펴보자.

```css
.media {
    display: flex;
    align-items: center;
    font-size: 16px;
    line-height: 1.5;
}
.media > figure {
    flex: 1 1 25%;
    margin-right: 3.33333%;
}
.media img {
    /* 이 스타일링은 범용적이므로 의도적으로 손자 셀렉터 상태로 사용함 */
    width: 100%;
    vertical-align: top;
}
.media body {
    flex: 1 1 68.33333%;
}
.media > h2 {
    margin-bottom: 10px;
    font-size: 18px;
    font-weight: bold;
}
.media > h3 {
    margin-top: 10px;
    margin-bottom: 10px;
    font-size: 16px;
    border-bottom: 1px solid #555;
}
.media-sub-text {
    color: #555;
    font-size: 14px;
}
```

이 상태로는 2장에서도 설명했던 '사용자를 고려한 설계로 만족스러운 체험을'이라는 형태는 그대로 유지하면서 'h2에서 h3로 변경하고 싶은' 문제를 해결할 수는 없다. 결국 스타일링에는 클래스 셀렉터를 사용하는 방법이 가장 안전하고 효율적이다.
