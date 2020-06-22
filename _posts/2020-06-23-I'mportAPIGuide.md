---
layout: post
title: '[아임포트] 아임포트 결제연동 API사용법'
subtitle: 아임포트 결제연동 API사용법 알아보기'
date: 2020-06-23
author: hansol Kim
image: '/assets/article_images/Import/iamport_main.png'
categories: Guide
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 아임포트 결제연동 API 사용 가이드(Vue.js)

- - -

### Link
* [I'mport; 결제연동 API 주소](https://www.iamport.kr/)

### 사용 이유
* 결제연동 테스트시 API를 무료로 사용할 수 있다.
* *카카오페이*, *KG이니시스*, *페이코* 등 다양한 결제방법 제공받을 수 있다.

### 회원가입
* [I'mport 페이지](https://www.iamport.kr/)에 접속하여 회원가입을 한다.

### script 등록
```
<!-- jQuery -->
<script type="text/javascript" src="https://code.jquery.com/jquery-1.12.4.min.js" ></script>
<!-- iamport.payment.js -->
<script type="text/javascript" src="https://cdn.iamport.kr/js/iamport.payment-1.1.8.js"></script>
```
* 위의 **script코드**를 등록한다.
* **vue.js**의 경우 **project경로/public/index.html**에 추가한다.

#### main.js에 코드추가
```
import IMP from "vue-iamport"
```
- - -
```
Vue.use(IMP, "[가맹점 고유코드 추가]")
Vue.IMP().load()
```
* main.js에서 vue-iamport부분을 import한다.
* import 후 두 번째 코드를 추가한다. 이 때, 가맹점 고유코드는 아임포트 홈페이지에서 우측 상단에 있는 **대시보드**를 클릭하게 되면 I'mport 관리자 페이지로 들어오게 된다.
* 관리자 페이지에서 **시스템설정 탭 클릭**  > **내정보** > **가맹점 식별코드** 에서 확인할 수 있다.

### 메서드에 코드 추가
```
Vue.IMP().request_pay({
  pg : 'inicis', // version 1.1.0부터 지원.
  pay_method : 'card',
  merchant_uid : 'merchant_' + new Date().getTime(),
  name : '주문명:결제테스트',
  amount : 14000,
  buyer_email : 'iamport@siot.do',
  buyer_name : '구매자이름',
  buyer_tel : '010-1234-5678',
  buyer_addr : '서울특별시 강남구 삼성동',
  buyer_postcode : '123-456',
  m_redirect_url : 'https://www.yourdomain.com/payments/complete'
}, (result_success) => { // 결제정보 서버 전송
  var msg = '결제가 완료되었습니다.';
  msg += '고유ID : ' + rsp.imp_uid;
  msg += '상점 거래ID : ' + rsp.merchant_uid;
  msg += '결제 금액 : ' + rsp.paid_amount;
  msg += '카드 승인번호 : ' + rsp.apply_num;
}, (result_failure) => {
  //실패시 실행 될 콜백 함수
  var msg = '결제에 실패하였습니다.';
  msg += '\n\n에러내용 : ' + result_failure.error_msg;
  alert(msg);
})
```
* 위의 예시코드를 메서드에 추가하여 테스트한다.
* 필요한 속성에 원하는 값을 넣어 사용한다. (ex. buyer_name: customer.name...)
* 결제 방법으로 카카오페이를 사용할 경우 pg : 'kakaopay'로 변경하여 사용한다. [param 속성 세부정보 확인](https://github.com/iamport/iamport-manual/blob/master/%EC%9D%B8%EC%A6%9D%EA%B2%B0%EC%A0%9C/README.md)

### PG설정
![PG설정 탭 화면](https://user-images.githubusercontent.com/31653025/85315794-ac117080-b4f6-11ea-8a27-fa248f7366da.JPG)
* 사용할 PG사를 선택한다.
* 형광으로 칠해진 부분은 테스트 모드를 적용한 부분이다. -> 테스트 모드 적용 시 실제 돈으로 결제되지 않는다. ~~*(카카오페이의 경우 가상으로 100만원이!)*~~

### 예시 API 적용 화면(카카오페이)
* 

### 결제 완료 후 결제내역 확인
![결제내역 확인](https://user-images.githubusercontent.com/31653025/85316087-1a563300-b4f7-11ea-9830-fcbd8e0b96c6.jpg)
* 관리자페이지에서 결제승인내역 탭을 선택하면 결제된 내역을 확인할 수 있다.

### 리뷰
* I'mport의 결제 API 적용방법은 상당히 다양하며 많은 기능을 가지고 있다. (ex. 본인인증, 네이버페이 상품구매평, 정기결제기능 등)
* 하지만 작성자가 직접 개발하면서 필요한 부분만을 적다보니 내용이 상당히 부실할 수 있다.
* 작성자도 추후 다양한 방법으로 사용하여 알게된 내용이 있을 경우 추가적으로 업로드 할 예정이다.
* 이 포스트가 결제 API를 적용해보는 데에 있어서 작은 도움이 되었기를 기대한다.
