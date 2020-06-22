---
layout: post
title: '[Daum 주소 API] 다음주소 API사용법'
subtitle: 다음주소 API사용 방법 알아보기'
date: 2020-06-23
author: hansol Kim
image: '/assets/article_images/daumAddress/address.JPG'
categories: Guide
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Daum 우편번호 서비스 사용 가이드

- - -

### Link
* [Daum 우편번호 API 주소](http://postcode.map.daum.net/guide#usage)

### script 추가
```
<script src="https://t1.daumcdn.net/mapjsapi/bundle/postcode/prod/postcode.v2.js"></script>
```
* **vue.js** 의 경우 **project경로/public/index.html** 에 추가한다.

### 메서드 구현
```
new daum.Postcode({
  oncomplete: function(data) {
      // 팝업에서 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분입니다.
      // data에서 필요한 속성 값을 이용하는 로직을 구현하는 부분
  }
}).open(); 
```

#### 예시(Vue.js)
```
new daum.Postcode({
  oncomplete: function(data) {
      customer.address = data.address // 팝업에서 클릭한 주소 저장
      customer.zonecode = data.zonecode // 팝업에서 클릭한 우편번호 저장
  }
}).open();  
```

### 메서드 연결
* 위에서 구현한 메서드를 **주소관련 form태그**에서 **onclick**(vue의 경우 @click)으로 메서드 이름을 등록하여 사용한다.

### 다음주소 API 적용화면
![다음주소 API 적용화면](https://user-images.githubusercontent.com/31653025/85317559-8174e700-b4f9-11ea-8eeb-4670294dc27c.JPG)

### 리뷰
* 상당히 간단한 편에 속하는 API이면서도 프론트에서 가장 많이 사용되는 API가 아닐까 싶다.