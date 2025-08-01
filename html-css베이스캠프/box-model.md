# CSS Box-Model
## CSS Box Model: 웹 레이아웃 기초.
모든 HTML 요소는 하나의 '박스'로 취급되며, 이 박스는 콘텐츠, 패딩, 테두리, 마진 구성된다. Box Model을 이해하면 웹 페이지의 구조를 더 정확하게 제어할 수 있다.

처음 레이아웃을 잡을 떄 이 박스 모델을 그려보는 것이 중요하다. 박스 모델을 그리면서 데이터를 어떻게 구조화하고, 그에 알맞은 태그를 무엇인지 생각해보아야 한다.

### 박스모델의 구조
요소: 텍스트, 사진 등 보여줄 대상
패딩: 요소 주변 영역을 감싼다.
테두리: 요소 패딩 감싸는 테두리입니다.
마진: 테두리 밖의 영역을 감싼다.

## width
요소의 너비를 설정한다. 기본값은 콘텐츠 영역의 너비이지만, `box-sizing` 속성을 사용하여 테두리 영역까지 포함할 수 있다. 너비를 입력하지 않으면 `auto`로 브라우저가 계산하여 지정한다. 이때, 부모 요소의 너비를 기준으로 계산하여 가득 채운다.

## height
요소의 높이를 설정한다. 입력하지 않으면 `auto`로 브라우저가 계산하여 지정한다.

이때, width 와는 다르게 content의 높이만큼만 설정된다.

## padding
패딩은 콘텐츠 주변의 여백을 만든다. 이 여백은 `border` 안쪽 여백이다.

작성 방향은 top -> right -> bottom -> left 이다.

개별 속성을 지정할 수도 있다.

## margin
마진은 요소 주변의 여백을 만든다. 마진은 border 바깥 여백이다.

패딩과 마찬가지로 설정가능.

## border

요소의 테두리를 지정한다. 테두리는 요소가 차지하는 전체 너비, 높이의 일부이다. 단축 속성이다. 다만, 이렇게 되었을 경우 일부 CSS에서 계산하기 어려운 경우가 있는데, box-sizing 속성을 사용하여 테두리를 포함하도록 설정할 수 있다.

이 속성은 선의 두께, 스타일, 색상을 지정할 수 있는 단축속성이다.

## box-sizing
`box-sizing` 속성은 요소의 총 너비와 높이를 계산하는 방식을 지정한다.

기본값은 `content-box`이다. `border-box`로 설정하면 테두리와 패딩을 포함하여 요소의 총 너비와 높이를 계산한다.

- `content-box`: 기본값: width, height에 border, padding 포함하지 않음.
- `border-box`: width, height에 border, padding 포함
    width = 콘텐츠 너비 + border + padding

## border-radius

