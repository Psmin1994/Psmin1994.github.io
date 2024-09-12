---
title: react-slick
author: Psmin
data: 2024-02-19 22:44:37 +0900
categories: [Knowledge, ReactJS]
tags: [Carousel]
---

# react-slick 라이브러리를 이용해 Carousel을 구현해보자.

---

## Carousel

Carousel이란 여러 개의 이미지, 비디오, 텍스트 등의 **콘텐츠를 일정한 간격으로 순서대로 보여주는 UI 요소**입니다.

![carousel-ex](/assets/img/carousel-ex.png){: .normal}

Carousel은 다양한 콘텐츠를 스크롤 없이 한 화면에서 제공할 수 있어 사용자들에게 더 많은 콘텐츠를 쉽게 제공할 수 있습니다.

> 공간 절약의 장점을 갖지만 성능 저하, 클릭 비율 등 어려 단점이 발생할 수도 있으니 주의해야합니다.

---

## react-slick

react-slick은 React 프로젝트에서 Carousel을 구현할 수 있는 라이브러리입니다.

- **설치**

```
npm i react-slick
npm i slick-carousel
```

사용할 Component에 기본 스타일 css파일을 import해서 사용합니다.

```jsx
import "slick-carousel/slick/slick.css";
import "slick-carousel/slick/slick-theme.css";
```

---

## settings 객체

settings 객체를 통해 Carousel의 형태나 컨텐츠를 보여줄 방식 등 여러 옵션을 설정합니다.

- **dots (boolean)** : 현재 위치를 나타내는 점들
- **autoplay (boolean)** : 자동 슬라이딩 기능
- **autoplaySpeed** : 자동 슬라이딩 이동 주기 (ms)
- **speed** : 수동 슬라이딩 속도
- **infinite** : 마지막 이미지 이후 다음 버튼 클릭 시, 첫 번째 이미지로 이동
- **slidesToShow** : 한번에 출력할 콘텐츠 수
- **slidesToScroll** : 한번에 넘어갈 콘텐츠 수
- **arrows** : 좌우 화살표를 표시할지 여부
- **prevArrow** : 이전 슬라이드를 보여주는 커스터마이징된 화살표 컴포넌트 설정
- **nextArrow** : 다음 슬라이드를 보여주는 커스터마이징된 화살표 컴포넌트 설정

> 그 외 더 많은 옵션 존재  
> 링크 : [Settings - API](https://react-slick.neostack.com/docs/api)

---

## 사용방법

공식문서의 샘플 코드를 보면 이해가 쉽습니다.

1. 먼저 settings 객체를 생성해 원하는 케러셀 옵션을 설정합니다.

2. Slider 컴포넌트에 설정한 옵션을 적용합니다.

3. Slider 컴포넌트 안에 보여줄 컨텐츠 요소들을 순서대로 정리합니다.

```jsx
import React from "react";
import Slider from "react-slick";

// 제공해주는 기본 스타일 적용
import "slick-carousel/slick/slick.css";
import "slick-carousel/slick/slick-theme.css";

export default function SimpleSlider() {
  // settings 객체를 통해 원하는 캐러셀 옵션을 설정합니다.
  var settings = {
    dots: true,
    infinite: true,
    speed: 500,
    slidesToShow: 1,
    slidesToScroll: 1,
    arrows: true,
  };

  return (
    <Slider {...settings}>
      <div>
        <h3>1</h3>
      </div>
      <div>
        <h3>2</h3>
      </div>
      <div>
        <h3>3</h3>
      </div>
      <div>
        <h3>4</h3>
      </div>
      <div>
        <h3>5</h3>
      </div>
      <div>
        <h3>6</h3>
      </div>
    </Slider>
  );
}
```

![react-slick-carousel-ex](/assets/img/react-slick-carousel-ex.png)

즉, react-slick 에서 제공하는 API를 통해 settings 객체를 수정해 사용자가 원하는 형태로 Carousel을 구성할 수 있습니다.

---

## 커스터마이징

형태뿐 아니라 CSS 스타일 또한 변경할 수 있습니다.

예시로 화살표 이미지를 변경해보겠습니다.

따로 css 파일을 만들어 기본 스타일에서 원하는 스타일을 오버라이딩 합니다.

```css
/* 기본 제공 스타일 */
@import "slick-carousel/slick/slick.css";
@import "slick-carousel/slick/slick-theme.css";

/* 아래에 새로운 스타일 오버라이딩 */

/* Arrows */
/* 기존의 화살표 삭제 */
.slick-prev:before,
.slick-next:before {
  display: none;
}

/* 사용할 화살표 이미지 크기에 맞게 오버라이딩 */
.slick-prev,
.slick-next {
  width: 30px;
  height: 30px;
}

.slick-prev {
  left: -50px;
}

.slick-next {
  right: -50px;
}
```

여기서 단점은 클래스 명을 확인하어 변경해주어야하기때문에 번거로울 수 있습니다.

```jsx
import React from "react";
import Slider from "react-slick";
import CustomArrow from "./CustomArrow";
import "css/Carousel.css"; // 추가적인 스타일이 오버라디이된 파일 import

export default function SimpleSlider() {
  // settings 객체를 통해 원하는 캐러셀 옵션을 설정합니다.
  var settings = {
    dots: true,
    infinite: true,
    speed: 500,
    slidesToShow: 1,
    slidesToScroll: 1,
    // 아래 두 옵션은 이전,다음 화살표에 사용할 컴포넌트를 지정할 수 있습니다.
    prevArrow: <CustomArrow direction="left" />,
    nextArrow: <CustomArrow direction="right" />,
  };

  return (
    <Slider {...settings}>
      <div>
        <h3>1</h3>
      </div>
      <div>
        <h3>2</h3>
      </div>
      <div>
        <h3>3</h3>
      </div>
      <div>
        <h3>4</h3>
      </div>
      <div>
        <h3>5</h3>
      </div>
      <div>
        <h3>6</h3>
      </div>
    </Slider>
  );
}
```

```jsx
// CustomArrow.jsx
import React from "react";

const CustomArrow = (props) => {
  const { className, style, direction, onClick } = props;

  return (
    <>
      <img
        className={className}
        style={{
          ...style,
        }}
        onClick={onClick}
        src={`${
          direction === "left"
            ? "/icons/arrow/left-arrow.png"
            : "/icons/arrow/right-arrow.png"
        }`}
        alt={`${direction === "left" ? "left-arrow" : "right-arrow"}`}
      />
    </>
  );
};

export default CustomArrow;
```
