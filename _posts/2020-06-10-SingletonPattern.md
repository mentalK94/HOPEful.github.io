---
layout: post
title: '[Design Pattern] 싱글턴 패턴'
subtitle: '싱글턴 패턴의 개념을 이해하고 구현할 수 있다.'
date: 2020-06-10
author: hansol Kim
image: '/assets/article_images/designPattern/designPattern.jpg'
categories: designPattern
tags: featured
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

# 싱글턴 패턴

## 싱글턴 패턴이란?
* 인스턴스를 하나만을 생성하여 사용하기 위한 패턴이다. 여러개의 인스턴스가 무의미한 상황인 한 개의 인스턴스만 필요한 경우 자원의 낭비나 버그의 발생을 줄일 수 있다.
하나의 인스턴스만을 유지하기 위해서 인스턴스 생성에 특별한 제약을 걸어야 한다. new연산자를 실행할 수 없도록 생성자에  private 접근 제어자를 지정하고,
유일한 단일 객체를 반환할 수 있도록 정적 메소드를 지원해야한다. 

## 일반적인 싱글턴 패턴
![그림 1](https://user-images.githubusercontent.com/31653025/63952879-45c8e680-cabb-11e9-80b6-3ddea7d4fd3b.PNG)

* 위 코드는 싱글턴 패턴 사용시 가장 많이 사용되는 코드이다. 일반적인 상황에서는 위 그림과 같이 Printer 클래스처럼 객체를 하나의 인스턴스로 만들어 사용해도 무방하다.

## 멀티스레드 사용시 문제점
![멀티스레드 사용시 결과](https://user-images.githubusercontent.com/31653025/63952915-511c1200-cabb-11e9-8042-0db011eb2f5f.PNG)

* 다중 스레드 환경에서 Printer 객체를 동시에 점유하려고 하면 [그림 1]과 같은 코드를 사용해도 여러개의 객체가 생성될 수 있다.
* 이유를 코드상에서 살펴보면 스레드 1이 getPrinter호출 시 인스턴스가 null값이므로 new연산자를 통해 인스턴스를 생성한다. 그런데 스레도 2도 스레드 1이 인스턴스를 만들기 전에 getPrinter호출하면 인스턴스가 null값이므로 생성하게 된다. 결과적으로 스레드 1, 2는 각각의 인스턴스를 생성하게 된다.

### solution 1. 동기화 사용
![동기화 사용](https://user-images.githubusercontent.com/31653025/63952919-537e6c00-cabb-11e9-9938-14d237138636.PNG)

![동기화 사용 결과](https://user-images.githubusercontent.com/31653025/63952921-55482f80-cabb-11e9-8276-154c1e0ab1d1.PNG)

* 코드를 보면 함수에 synchronized를 사용하여 메서드 동기화를 한다. 따라서 결과 이미지를 보면 다중 스레드가 같은 객체를 공유함을 볼 수 있다.

### solution 2. 정적변수 사용
- (코드) private static Printer printer = new Printer(); 이렇게 코드를 사용하여 printer라는 정적변수를 이용한다
- (주의점) 미리 생성해놓고 사용될 때 가져다 사용하는 방식으로 사용하지 않아도 메모리 점유하고 있으므로 메모리의 낭비가 있다

## LazyHolder
- ![lazyholder](https://user-images.githubusercontent.com/31653025/64708206-313f1200-d4ef-11e9-9795-eec0482bb24c.PNG)

* 이 방법은 JAVA버전과 무관하게 성능이 뛰어난 방법이다. static영역에 초기화를 하지만 객체가 필요한 시점까지 최대한 초기화를 미루는 방식이다.
* LazyHolder.INSTANCE를 참조하는 순간 Class가 로딩되며 초기화가 진행되고 이 시점에는 thread-safe를 보장하기 때문에 volatile이나 synchronized
* 키워드 없이 성능도 보장한다.

# References
> - [https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)
> - [ JAVA 객체지향 디자인 패턴 정인상, 채흥석 저 ]