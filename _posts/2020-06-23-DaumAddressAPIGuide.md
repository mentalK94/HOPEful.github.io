---
layout: post
title: '[Daum 주소 API] 다음주소 API사용법'
subtitle: 다음주소 API사용 방법 알아보기'
date: 2020-06-23
author: hansol Kim
image: '/assets/article_images/daumAddress/address.jpg'
categories: Guide
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## Daum 우편번호 서비스 사용 가이드

### 참조 주소
* [https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)

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

### 3. 결과화면(추후 업로드 예정)