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

하지만 이 상태에서는 버튼을 main
