---
layout: post
title: '[Design Pattern] 스트래티지 패턴'
subtitle: '스트래티지 패턴의 개념을 이해하고 구현할 수 있다.'
date: 2020-06-11
author: hansol Kim
image: '/assets/article_images/designPattern/designPattern.jpg'
categories: Design Pattern
tags: featured
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---



## 스트래티지 패턴이란?

* 행위를 클래스로 캡슐화하여 동적으로 행위를 자유롭게 바꿀 수 있는 패턴이다. 즉, 전략을 쉽게 바꿀 수 있도록 해주는 디자인 패턴이다.

## 스트래티지 패턴 클래스 다이어그램
![스트래티지 패턴 클래스 다이어그램](https://user-images.githubusercontent.com/31653025/83719238-9d5f3880-a671-11ea-8d15-ea0d2153bede.PNG)

* Strategy : 인터페이스나 추상클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시

* ConcreteStrategy : 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스

* Context : 스트래티지 패턴을 이용하는 역할을 수행하며, ConcreteStrategy를 바꿀 수 있도록 setter메서드(집약 관계)를 제공

## 로봇만들기 예시

### 기본 로봇 설계
![기존 설계](https://user-images.githubusercontent.com/31653025/83719648-7f460800-a672-11ea-8129-af3a16180ae2.PNG)

* 로봇을 추상화하여 자식클래스에서 각각 정의하도록 설계

### 문제점
* 위와 같은 설계는 2가지 문제점이 발생한다.
* (문제점 1) 로봇이 적으면 문제가 되지 않을 수 있지만 수백~수천개 이상이 되는 경우 모든 로봇의 move와 attack메소드를 정의해야한다.
* (문제점 2) 공격과 이동외에 새로운 전략이 추가되거나 수정될 경우 기존의 코드를 수정하게 되어 **OCP 원칙**을 위배하게 된다.

### 해결책
* 무엇이 변화되었는지 찾아야한다.
* 변화된 것을 클래스로 캡슐화해야 한다.
* **이동**과 **공격** 방법이 변화되었기 때문에 이를 클래스로 캡슐화한다.

### 공격과 이동전략 인터페이스 설계
![공격과 이동전략 인터페이스 설계](https://user-images.githubusercontent.com/31653025/83720447-0ba4fa80-a674-11ea-853d-a0172d7c2f0c.PNG)

### 개선된 설계
![개선된 설계 다이어그램](https://user-images.githubusercontent.com/31653025/83720482-1d869d80-a674-11ea-8095-a19b5efc6ca9.PNG)

![Context에서 Setter생성](https://user-images.githubusercontent.com/31653025/83720534-2f684080-a674-11ea-9b06-4d9d281b42ff.png)

* Context인 **Robot**에서 **Strategy**를 사용할 수 있도록 **setter메소드**가 필요하다.
* 이렇게 변경된 구조는 향후 등장할 이동 방식과 공격 방식의 변화뿐만 아니라 현재 변화도 잘 처리할 수 있게 된다.
* Strategy Interface가 변화에 대한 일종의 **방화벽**역할을 수행하여 Robot클래스의 변경을 차단한다.(OCP만족하는 설계)

## 참조
> - [https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)
> - [JAVA 객체지향 디자인 패턴 정인상, 채흥석 저]