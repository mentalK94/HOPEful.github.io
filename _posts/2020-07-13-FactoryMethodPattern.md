---
layout: post
title: '[Design Pattern] 팩토리 메서드 패턴'
subtitle: 팩토리 메서드 패턴의 개념을 이해하고 구현할 수 있다.'
date: 2020-07-13
author: hansol Kim
image: '/assets/article_images/designPattern/designPattern.jpg'
categories: DesignPattern
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 팩토리 메서드 패턴이란?

* 팩토리 메서드 패턴은 객체를 생성하는 코드를 별도의 클래스/메서드를 분리함으로써 객체 생성 방식의 변화에 대비하는 데 유용하다.

## 여러가지 장식의 엘리베이터 스케줄링 방법 지원하기

### 기본 설계
![작업처리량을 기준으로 한 스케줄링에 따른 엘리베이터 관리](https://user-images.githubusercontent.com/31653025/87274377-96e89a00-c516-11ea-9323-fbb573c77eb4.JPG)

### 문제점
* (문제 1) 현재 *ElevatorManager* 클래스는 *ThroughputScheduler* 클래스를 이용한다. 만약 다른 스케줄링 전략을 사용해야 한다면?
* (문제 2) 프로그램 실행 중에 스케줄링 전략을 변경, 즉 동적 스케줄링을 지원해야 한다면? 예를 들어 오전에는 대기 시간 최소화 전략을 사용하고, 오후에는 처리량 최대화 전략을 사용해야 한다면?

### 문제 1 - 다른 스케줄링 전략을 사용
![스트래티지 패턴을 활용한 엘리베이터 스케줄링 전략 설계](https://user-images.githubusercontent.com/31653025/87274372-95b76d00-c516-11ea-88f7-f0927b4ec60f.JPG)

* **스트래티지 패턴** 을 사용하게 되면서 *ThroughputScheduler* 또는 *ResponseScheduler* 중 한 클래스를 동적으로 선택할 수 있게됨.


### 문제 2 - 동적 스케줄링을 지원해야 한다면?(ex. 오전에는 ResponseTime, 오후에는 Throughput을 사용하는 경우)

~~~ java
/* ElevatorManager class중 일부분 */

void requestElevator(int destination, Direction direction) {
	ElevatorScheduler scheduler;

	int hour = Calender.getInstance().get(Calendar.HOUR_OF_DAY);

	if(hour < 12)
		scheduler = new ResponseTimeScheduler();
	else
		scheduler = new ThroughputScheduler();
}
~~~

* 스케줄링 전략이 변경될 때마다 *ElevatorManager* 에서 *requestElevator* 메서드도 수정되어야 하는 문제가 발생.

### 해결책
* 이러한 문제를 해결하기위해 주어진 기능을 실제로 제공하는 적절한 클래스 생성 작업을 별도의 **클래스/메서드** 로 분리시키는 편이 좋다.


### 개선된 설계
![팩토리 메서드를 이용한 스케줄링 전략 객체의 생성](https://user-images.githubusercontent.com/31653025/87274379-96e89a00-c516-11ea-84f5-02d5acb210cf.JPG)
* *getScheduler* 메서드가 스케줄링 전략에 맞는 객체를 생성하는 기능을 구현한 코드이다.

~~~ java
package framework;

public class SchedulerFactory {

	public static ElevatorScheduler getScheduler(SchedulingStrategyID strategyID) {
		
		ElevatorScheduler scheduler = null;
		switch (strategyID) {
		case RESPONSE_TIME:
			scheduler = ResponseTimeScheduler.getInstance();
			break;
		case THROUGHPUT:
			scheduler = ThroughputScheduler.getInstance();
			break;
		case DYNAMIC:
			int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
			if (hour < 12) { // 오전			
				scheduler = ResponseTimeScheduler.getInstance();
			}
			else {
				scheduler = ThroughputScheduler.getInstance();
			}
			break;
		}
		return scheduler;
	}
}
~~~

* 객체 생성시 앞서서 배웠던 **싱글턴 패턴** 을 적용하여 객체를 생성 및 사용하고 있다.
* 의문) 팩토리 메서드 패턴을 사용하여 *getScheduler* 메서드를 구현했는데 만약 스케줄링 전략이 추가되면 *getScheduler* 코드역시 수정해야하는 것 아닌가..추후 이해하게 되면 수정본 업로드 예정

### 일반적인 데커레이터 패턴
![팩토리 메서드 패턴의 일반적인 클래스 다이어그램](https://user-images.githubusercontent.com/31653025/87276842-19c02380-c51c-11ea-92e6-f64ffea3faf2.JPG)

* **Product** : 팩토리 메서드로 생성될 객체의 공통 인터페이스
* **ConcreteProduct** : 구체적으로 객체가 생성되는 클래스
* **Creator** : 팩토리 메서드를 갖는 클래스
* **ConcreteCreator** : 팩토리 메서드를 구현하는 클래스로 *ConcreteProduct* 객체를 생성한다.

## 참조
> - [JAVA 객체지향 디자인 패턴 정인상, 채흥석 저]