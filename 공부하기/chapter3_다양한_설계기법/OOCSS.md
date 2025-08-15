# OOCSS

Object Oriented CSS(객체 지향 CSS)의 약어이다.

-   레고처럼 자유로운 조합이 가능한 모듈의 집합을 만든다.
-   그 모듈을 조합해 페이지를 만든다.
-   그리하여 신규 페이지를 만드는 경우에도 기본적으로 추가로 CSS를 만들 필요가 없다.

위와 같은 발상으로 제창된 OOCSS는 다른 CSS 설계 기법에도 조금씩 영향을 주었다. 이 레고와 같은 모듈을 구현하기 위해 구체적인 수법으로 다음 두 가지 원칙을 들 수 있다.

-   스트럭처(구조)와 스킨(화면) 분리
-   콘테이너와 콘텐츠 분리

## 스트럭처와 스킨 분리

`8. 확장하기 쉽다`

두 가지 다른 버튼이 있다고 가정해 봅시다.
이때, 버튼은 구조(가로 길이나 높이 등)와 형태(박스의 그림자나 배경 색상, 문자 색상 등)를 조합해서 만들어진다'고 생각하는 것이 '스트럭처와 스킨을 분리'하는 사고방식이다.

이때, [스트럭처]에 해당하는 속성은 다음과 같다.

-   width
-   height
-   padding
-   margin

[스킨]에 해당하는 속성은 다음과 같다.

-   color
-   font
-   background
-   box-shadow
-   text-shadow

이렇게 구분할 수 있지만 OOCSS에서는 명확하게 결정되어 있는 것은 아니다. 이 부분은 너무 이론에 얽매이지 말고 경우에 따라 적절하게 분류하는 방법을 사용해도 무방하다.

```css
#main .btn-general {
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    background-color: #e25c00;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
    color: #fff;
    font-size: 18px;
    line-height: 1.5;
    text-align: center;
}

#main .btn-warning {
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

=>

```css
/* 스트럭처 */
#main .btn {
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, .16);
    font-size: 18px;
    line-height: 1.5;
    text-align
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

위의 코드를 아래처럼 정리!

다른 색상의 버튼을 추가하더라도 단지 몇 줄의 코드를 삽입하는 것만으로 즉시 구현할 수 있다.

## 컨테이너와 콘텐츠 분리

`4. 특정한 콘텍스트에 지나치게 의존하지 않는다.`

이번에는 컨테이너와 콘텐츠의 분리에 관해 알아보자. 컨테이너는 대략 '영역', 콘텐츠는 바로 앞절에서 본 '버튼' 모듈을 생각하면 된다.

에를 들어, 바로 앞의 예시에서는 버튼 모듈은 id 속성에 main이 지정된 main 요소 안에 포함되어 있다. 하지만 이 상태에서는 버튼을 main 밖에서 사용하려 해도 그럴 수 없다. 이 문제에 대한 해결 방법은 매우 간단하다. 버튼 모듈을 main 밖에서도 동작하도록 CSS 셀렉터를 수정한다. 컨테이너와 콘텐츠의 분리라는 것은 다시 말해 `모듈을 가능한 특정한 영역에 의존하지 않도록 한다.`는 지침을 의미한다.

```css
/* 스트럭처 */
.btn { /* main의 ID 셀렉터를 삭제한다. */
    display: inline-block;
    width: 300px;
    max-width: 100%;
    padding: 20px 10px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, .16);
    font-size: 18px;
    line-height: 1.5;
    text-align
}

/* 스킨 */
.general { /* main의 ID 셀렉터를 삭제한다. */
    background-color: #e25c00;
    color: #fff;
}

.warning { /* main의 ID 셀렉터를 삭제한다. */
    background-color: #f1de00;
    color: #222;
}
```

## OOCSS 정리

지금까지 OOCSS의 대원칙인 두 가지 사고방식에 관해 설명했다. OOCSS의 역사는 매우 길며 명확하게 규칙이라고 불리는 것도 많지 않다.

이제부터 설명할 CSS 설계 기법들은 기본적으로 크건 작건 OOCSS를 참조하면서 개선을 한 것이다.

그러나 10년 전 제창한 사고방식이 다른 CSS 설계 기법에 녹아들어, 지금까지도 사용되는 것을 생각하면 OOCSS가 표방했던 사고는 CSS 설계에 있어 '하나의 진리'라고 해도 과언이 아니다. OOCSS는 CSS 설계의 기초 중의 기초이다. 이 내용들을 꼭 기억하자.
