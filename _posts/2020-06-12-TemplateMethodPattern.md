---
layout: post
title: '[Design Pattern] 템플릿 메소드 패턴 패턴'
subtitle: '싱글턴 패턴의 개념을 이해하고 구현할 수 있다.'
date: 2020-06-10
author: hansol Kim
image: '/assets/article_images/designPattern/designPattern.jpg'
categories: DesignPattern
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

# 템플릿 메소드 패턴

## 템플릿 메소드 패턴이란?

작업을 처리하는 일부분을 서브클래스로 캡슐화하여 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 수정하는데 용이한 패턴

### 코드확장 시 문제점
- HyundaiMotor뿐만 아니라 Samsung, LG모터도 관리해야하는 경우 코드 추가시 중복코드가 발생한다.
- 따라서 아래 그림과 같이 Motor라는 상위클래스를 만들어 상속받아 사용한다.
![templateMethod-Diagram1](https://user-images.githubusercontent.com/31653025/72790792-92cf7400-3c79-11ea-9cdb-6a689b902d3b.PNG)
[다이어그램1]

### 부분적으로 중복되는 경우
- 현상

![templateMethod-move(HyundaiMotorClass)](https://user-images.githubusercontent.com/31653025/72792253-0bcfcb00-3c7c-11ea-88d8-78c35d78457a.PNG)
[HyundaiMotor의 move메서드]
![templateMethod-move(LGMotorClass)](https://user-images.githubusercontent.com/31653025/72792256-0d998e80-3c7c-11ea-8baa-fdf70d382de3.PNG)
[LGMotor의 move메서드]
![templateMethod-move(SamsungMotorClass)](https://user-images.githubusercontent.com/31653025/72792261-0f635200-3c7c-11ea-92a0-971f36e00072.PNG)
[SamsungMotor의 move메서드]

- 위 코드들을 보면 move***Motor()를 제외하고는 코드가 중복된다.

- 해결책
![templateMethod-Diagram2](https://user-images.githubusercontent.com/31653025/72793746-33c02e00-3c7e-11ea-9879-0de14c3d9eef.PNG)
[다이어그램2]
- 위에 다이어그램과 같이 추상클래스에서 move메서드를 구현한다.
- 여기서 <b>move메서드</b>를 <b>Template Method</b>라고 부르며 <b>moveMotor메서드</b>를 <b>Primitive Method</b>또는 <b>Hook Method</b>라 부른다.
- <b>Primitive Method</b>또는 <b>Hook Method</b>를 구현하는 클래스를 <b>Concrete클래스</b>라 부른다.

## 언제 사용하는가?
- 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화 할 때 유용
- 동일한 기능을 상위 클래스에서 정의하면서 확장/변화가 필요한 부분만 서브 클래스에서 구현
- 전체적인 알고리즘은 상위클래스에서 구현하면서 다른 부분은 하위클래스에서 구현할 수 있도록 하여 전체적인 알고리즘 코드를 재사용하는데 유용

## 참조 
- https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html
- Java 객체지향 디자인패턴(채흥석, 정인상 저)
