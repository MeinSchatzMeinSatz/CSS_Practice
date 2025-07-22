# 3-1 이 장의 설명에 앞서

이번 장에서는 웹 업계에서 실제 사용하고 있는 여러 CSS 설계 기법을 소개하고 설명한다. 한데 각 설계 기법에 대해 공식 문서에 실려 있는 모든 사항을 설명하려면 그 분량이 너무 방대해진다. 그러므로 이 책에서는 각 설계 기법에서의 중요한 사고방식만 골라내서 설명한다. 특히, 빌드 환경이나 CSS 전처리기에 관해서는 다루지 않으므로 실제 프로젝트에 도입하기 위해 보다 상세한 정보를 알고 싶은 분은 공식 문서도 함께 읽어라.

이번 장에서 제시하는 예시 코드는 쉽게 설명하는 것을 우선하고 있으므로 다음의 처리를 적용하였다.

-   주제와 관련 없는 코드는 생략
-   알기 쉬운 값을 예시에 사용.

더불어 각 절의 내용을 설명하며 2장의 'CSS 설계 실전과 여덟 가지 포인트'에서 소개한 것과 관련 있는 내용은 절 제목 아래 '연관 포인트'라고 표기한다. 특히, 2장과 거의 동일한 형태의 문제를 다루는 경우 끝에 ⭐️을 붙였다.

# OOCSS

1장과 2장에서도 잠깐 이름이 등장했던 OOCSS는 Object Oriented CSS(객체 지향 CSS)의 약어다.

-   레고처럼 자유로운 조합이 가능한 모듈의 집합을 만든다.
-   그 모듈을 조합해 페이지를 만든다.
-   그리하여 신규 페이지를 만드는 경우에도 기본적으로 추가로 CSS를 만들 필요가 없다.

위와 같은 발상으로 제창된 OOCSS는 다른 CSS 설계 기법에도 조금씩 영향을 주었다. 이 레고와 같은 모듈을 구현하기 위한 구체적인 수법으로 다음 두 가지 원칙을 들 수 있다.

-   스트럭처(구조)와 스킨(화면) 분리
-   컨테이너와 콘텐츠 분리

## 스트럭처와 스킨 분리

8. 확장하기 쉽다(⭐️)
   '스트럭처와 스킨 분리'에 관해서는 2장에서 소개한 CSS 설계의 여덟 가지 포인트 중 '8장 확장하기 쉽다'에서 설명한 싱글 클래스 설계와 멀티 클래스 설계가 매우 유사하다. 그림 3-1과 같은 두 종류의 버튼이 있다고 가정해보자.

기본 버튼 vs 취소 버튼

특별한 고민 없이 별도 클래스로 구현하면 코드는 다음과 같다.

```html
<main id="main">
    <button class="btn-general">기본 버튼</button>
    <button class="btn-warning">취소 버튼</button>
</main>
```

```css
#main .btn-general {
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    background-color: #e25c00;
    color: #fff;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
    color: #fff;
    font-size: 18px;
    line-height: 1.5;
    text-align: center;
}
main .btn-warning {
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    background-color: #f1de00;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
    color: #222;
    font-size: 18px;
    line-height: 1.5;
    text-align: center;
}
```

color와 background-color 외에는 동일하다. 버튼 구성을 보면 '버튼은 구조와 형태를 조합해서 만들어진다' 고 생각하는 것이 '스트럭처와 스킨을 분리'하는 사고방식이다.(구조와 형태)

스트럭처에 해당하는 속성은 크게 다음과 같다.

-   width
-   height
-   padding
-   margin

스킨에 해당하는 속성은 크게 다음과 같다.

-   color
-   font
-   background
-   box-shadow
-   text-shadow

위와 같이 구분할 수 있지만 OOCSS에서는 명확하게 결정되어 있는 것은 아니다. 이 부분은 너무 이론에 얽매이지 말고 경우에 따라 적절하게 분류하는 방법을 사용해도 무방하다. 앞의 코드를 기준으로 말하면 공통된 부분은 스트럭처, 공통되지 않은 부분을 스킨이라고 하면 괜찮을 것이다.

스트럭처와 스킨 분리 에 따라 수정한 코드는 다음과 같다.

```html
<main id="main">
    <button class="btn general">기본 버튼</button>
    <button class="btn warning">취소 버튼</button>
</main>
```

```css
/* 스트럭처 */
#main . btn {
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
    font-size: 18px;
    line-height: 1.5;
    text-align: center;
}
/* 스킨 */
#main .general {
    background-color: #e25c00;
    color: #fff;
}
#main .warning {
    background-color: #f1de00;
    color: #222;
}
```

이제 다른 색상의 버튼을 추가하더라도 단지 몇 줄의 코드를 삽입하는 것만으로 즉시 구현할 수 있다.

## 컨테이너와 콘텐츠 분리

4. 특정한 콘텍스트에 지나치게 의존하지 않는다(⭐️)

이번에는 컨테이너와 콘텐츠 분리에 관해 알아보겠다. 컨테이너는 대략 '영역', 콘텐츠는 바로 앞절에서 본 '버튼' 모듈을 생각하면 된다. 예를 들어, 바로 앞의 예시에서는 버튼 모듈은 id 속성에 main이 지정된 main 요소 안에 포함되어 있다.

```html
<main id="main">
    <button class="btn general">기본 버튼</button>
    <button class="btn warning">취소 버튼</button>
</main>
```

```css
/* 스트럭처 */
#main . btn {
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
    font-size: 18px;
    line-height: 1.5;
    text-align: center;
}
/* 스킨 */
#main .general {
    background-color: #e25c00;
    color: #fff;
}
#main .warning {
    background-color: #f1de00;
    color: #222;
}
```

하지만 이 상태에서는 버튼을 main 밖에서 사용하려 해도 그럴 수 없다. 이 문제에 대한 해결 방법은 매우 간단하다. 버튼 모듈을 main 밖에서도 동작하도록 CSS 셀렉터를 수정한다. 컨테이너와 콘텐츠의 분리라는 것은 다시 말해 '모듈을 가능한 특정한 영역에 의존하지 않도록 한다'는 지침을 의미한다.

```html
<main id="main">
    <button class="btn general">기본 버튼</button>
    <button class="btn warning">취소 버튼</button>
</main>

<footer>
    <button class="btn general">기본 버튼</button>
</footer>
```

```css
/* 스트럭처 */
.btn {
    /* #main의 ID 셀렉터를 삭제함 */
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
    font-size: 18px;
    line-height: 1.5;
    text-align: center;
}
/* 스킨 */
#main .general {
    /* #main의 ID 셀렉터를 삭제함 */
    background-color: #e25c00;
    color: #fff;
}
#main .warning {
    /* #main의 ID 셀렉터를 삭제함 */
    background-color: #f1de00;
    color: #222;
}
```

### OOCSS 정리

지금까지 OOCSS의 대원칙인 두 가지 사고방식에 대해 설명했다. OOCSS의 역사는 매우 길며 명확하게 규칙이라고 불리는 것도 많지 않다.(공식 사이트를 보면 알 수 있지만 설명도 매우 간략하다.) 사실 이제부터 설명할 CSS 설계 기법들은 기본적으로 크건 작건 OOCSS를 참조하면서 개선을 한 것이다. 오늘날 OOCSS 한 가지만으로 실질적인 CSS 설계를 수행하는 것은 그다지 현실적이지 않다.

그러나 10년 전 제창한 사고방식이 다른 CSSㅅ 설계 기법에 녹아들어, 지금까지도 사용되는 것을 생각하면 OOCSS가 표방했던 사고는 CSS 설계에 있어 '하나의 진리'라고 해도 과언이 아니다. OOCSS는 CSS 설계의 기초 중의 기초이므로 이 내용들을 꼭 기억하자.

## SMACSS

SMACSS(스맥스)는 Scalable and Modular Architecture for CSS(CSS를 위한 확장 가능한 모듈 아키텍처)의 약어다. CSS 코드를 그 역할에 따라 분류한 것이 특징으로 다음 다섯 가지 분류에 따라 각각 규칙을 설정하고 있다.

1. 베이스(Base)
2. 레이아웃(Layout)
3. 모듈(Module)
4. 스테이트(State)
5. 테마(Thema)

OOCSS에서 말한 범위는 SMACSS의 '모듈'과 거의 비슷하다. OOCSS가 거의 모듈만 언급했던 것에 비해 SMACSS는 보다 폭넓고 실제로 웹사이트를 구축하는 데 빼놓을 수 없는 베이스나 레이아웃 코드를 다루는 방법까지 말하고 있다.

### 베이스 규칙

1. 특성에 따라 CSS를 분류한다.(⭐️)

베이스 규칙에서는 프로젝트의 표준 스타일을 정의한다. 베이스 규칙에서 사용하는 셀렉터는 주로 요소형 셀렉터(body {})이지만 그 외에 다음 셀렉터도 사용한다.

-   자녀 셀렉터(a > img {}등)
-   손자 셀렉터(ul li {}등)
-   의사 클래스(a:hover {}등)
-   속성 셀렉터(a[href] {}등)
-   인접 형제 셀렉터(h2 + p {}등)
-   일반 형제 셀렉터(h2 ~ p {}등)

그러나 기본 규칙을 너무 많이 정의해 놓으면 영향 범위가 넓어져 CSS 설계 포인트 중'3. 영향 범위를 지나치게 넓히지 않는다'를 위반하게 되므로 주의하자. 또한 '프로젝트 내에서 각 요소가 표준으로 어떻게 동작하느가"를 정의하기 위한 규칙이기 떄문에, 특정한 상황에서의 사용을 가정하는 ID 셀렉터나 클래스 셀렉터에는 사용할 수 없다. 같은 이유로 기본 규칙에서는 !important도 사용하지 않는다.

코드는 대체로 다음과 같다.

```css
body {
    background-color: #fff;
}
/* 자녀 셀렉터 예시 */
a > img {
    transition: 0.25s;
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

SMACSS에서는 리셋 CSS 베이스 규칙으로 포함한다. 2장에서 소개한 웹에 공개되어 있는 기존의 리셋 CSS 사용하는 경우에는 그만큼 코드가 길어질 것이다. 또한 규칙이라 할 수는 없지만 SMACSS에서는 보디(Body)의 배경 색상을 기본 규칙으로 설정하는 것을 강하게 권장한다. 이는 웹사이트를 보는 사용자가 브라우저 기능을 사용해 배경색을 독자적으로 지정한 경우, 색상에 따라 웹사이트가 정상적으로 보이지 않을 가능성이 있기 때문이다.

### 레이아웃 규칙

1. 특성에 따라 CSS를 분류한다.(⭐️)

레이아웃 규칙은 헤더나 메인 영역, 사이드 바, 푸터 등 웹사이트의 큰 틀을 구성하는 큰 모듈에 관한 규칙이다. 레이아웃을 구성하는 것의 대부분은 특정 페이지에서 한 차례만 사용되는 것이 많으므로 ID 셀렉터를 활용한 스타일링을 허용한다. 레이아웃과 관련해서 반복적으로 사용하느 모듈의 경우에는 클래스 셀렉터를 이용한다.

다음 코드는 레이아웃 규칙 예시다.

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
    margin-left: aoto
    background-color: #fff
}
#footer {
    width: 1080px;
    margin-right: auto;
    margin-left: aoto
    background-color: #eee
}
.section {
    padding-top: 80px;
    padding-bottom:80px;
}
```

#### 특정한 상황에서 레이아웃이 변경되는 경우

예를 들어, '특정한 페이지에서는 가로 폭을 좁히고 싶은' 경우에는 손자 셀렉터를 이용해 레이아웃 모듈의 스타일을 덮어쓸 수 있다. 다음 코드에서는 body 요소에 .l-narrow 클래스를 붙여서 손자 셀렉터를 사용해 헤더, 메인 영역, 푸터의 가로 폭을 좁혔다.

```css
.l-narrow #header {
    width: 960px;
}
.l-narrow #main {
    width: 960;
}
.l-narrow #footer {
    width: 960px;
}
```

이 코드에서 주목할 부분은 .l-narrow에서 접두사 'l-'이 붙어 있다는 있다는 점이다. 레이아웃 규칙에서 클래스 셀렉터를 사용할 때는 'l-'이라는 접두사를 붙이는 것을 권장한다. 이는 뒤에서 설명할 모듈 규칙이나 스테이트(state) 규칙에 해당하는 모듈과 쉽게 구분할 수 있게 하기 위한 조치이다. 그런 관점에서 보자면 앞에서의 .section이라는 클래스 이름 역시 SMACSS에서는 .l-section이라는 이름으로 바꾸는 것이 바람직하다.

```css
/* 클래스 셀렉터 예시 */
.section {
    padding-top: 80px;
    padding-top: 80px;
}
.l-section {
    padding-top: 80px;
    padding-bottom: 80px;
}
```

주의할 것은 이 접두사는 클래스 셀렉터 뿐만 아니라 ID셀렉터에서도 사용할 수 있다는 것이다. 하지만 ID 셀렉터는 레이아웃 규칙에서만 사용되며 한 페이지 안에서의 사용 빈도도 그리 높지 않으므로 접두사를 붙이지 않아도 충분히 구현할 수 있을 것이다. SMACSS에서는 이 부분의 규칙이 엄격하지 않기 때문에 어떤 방법을 사용하는 것이 좋을지 고민스러울 것이다.

필자의 경우에는 모든 클래스 셀렉터에 통일하는 것이 알기 쉽기 때문에 이 규칙을 선호한다.

ID 셀렉터를 클래스 셀렉터로 변환한 경우의 CSS 예시다.

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
    background-color: #fff;
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

#### 모듈 규칙

모듈 규칙에 해당하는 모듈이란, 앞에서 설명한 레이아웃 모듈 안에 배치되는 것을 가정하고 있다.

-   타이틀(Title)
-   버튼(Button)
-   카드(Card)
-   네비게이션(Navigation)
-   캐러셀(Carousel)

위와 같이 레이아웃 모듈 안에 배치할 수 있는 개별 모듈이라면 SMACSS에서는 모두 모듈 규칙에 해당한다.

모듈은 다른 페이지로 이동하거나 다른 레이아웃 안에 삽입하더라도 형태가 부서지거나 달라지지 않고 사용할 수 있어야 한다. CSS 설계의 허리가 되는 부분이기도 하므로 구현 시에는 '불필요한 코드는 없는가', '이 코드는 다른 레이아웃으로 이동했을 때 영향이 없는가' 등 한층 주의가 필요하다. 한 페이지 내에서 반복해서 사용되는 상황을 가정하고 있으므로 당연히 ID셀렉터에서의 구현은 하지 않으며, 모듈 루트 요소에는 반드시 클래스 셀렉터(HTML에서는 클래스 속성)를 사용한다.

모듈을 만들 떄는 다음과 같은 두 가지 사항에 주의해야 한다.

-   가급적 요소형 셀렉터를 사용하지 않는다.
-   특정한 콘텍스트에 지나치게 의존하지 않는다.

이 사항들은 SMACSS 공식 문서에서도 다루고 있으므로 각각 설명하자.

##### 가능한 요소형 셀렉터를 사용하지 않는다.

2. HTML과 스타일링을 느슨하게 결합한다(⭐️)

2장 CSS 설계의 여덟 가지 포인트 중 '2. HTML과 스타일링을 느슨하게 결합한다'에 설명한 것과 완전히 같은 이야기다. 2장에 이어서 여기에서도 모듈 예시로서 그림 3-2 미디어 모듈을 사용한다.
