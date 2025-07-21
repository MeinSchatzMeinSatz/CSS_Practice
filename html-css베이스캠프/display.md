# display 속성

## 레이아웃의 핵심

`display` 속성은 CSS에서 레이아웃을 결정짓는 중요한 속성 중 하나이다. 이 속성은 요소가 문서 흐름에서 다른 박스들과 어떤 방식으로 배치할지 어떻게 배치할지를 결정한다.

-   display 속성의 주요 값들
    display: block
    display: inline
    display: inline-block

display: flex
display: grid
display: none

### block

-   요소를 블록 레벨의 요소로 만든다.
-   새로운 줄에서 시작하며, 가능한 전체 너비를 차지한다.
-   width, height, margin, padding 속성을 모두 사용할 수 있다.

*   span과의 비교.

### inline

-   요소를 인라인 요소로 만든다.
-   텍스트 흐름에 따라 배치되며, 줄 바꿈 없이 다른 요소와 같은 줄에 위치할 수 있다.
-   요소의 너비는 내용에 맞게 설정된다.
-   width, height 속성을 사용할 수 없으며, margin 속성은 좌우만 적용된다.

### inline-block

-   인라인 요소와 블록 요소의 특성을 결합한다.
-   인라인처럼 텍스트 흐름에 따라 배치되지만, 블록처럼 width, height, margin, padding을 모두 지정할 수 있다.

### none

요소를 완전히 숨긴다. 요소가 렌더링 되지 않으며, 레이아웃에서 공간을 차지 하지 않는다.

## 주의사항

display 속성으로 요소의 시각적 표현을 변경할 수는 있지만, 이는 HTML 문서의 구조나 의미를 변경하지 않는다. 그렇기 때문에 항상 콘텐츠의 의미와 구조를 고려해서 개발할 필요가 있다. CSS는 콘텐츠의 시각적인 표현을 담당하는 것이지, 콘텐츠의 의미나 구조를 변경하는 것이 아니다.
