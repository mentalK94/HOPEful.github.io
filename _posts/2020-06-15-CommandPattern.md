---
layout: post
title: '[Design Pattern] 커맨드 패턴'
subtitle: 커맨드 메소드 패턴의 개념을 이해하고 구현할 수 있다.'
date: 2020-06-15
author: hansol Kim
image: '/assets/article_images/designPattern/designPattern.jpg'
categories: DesignPattern
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 커맨드 패턴이란?

* 이벤트가 발생했을 때 실행될 기능이 다양하면서도 추가 또는 변경이 필요한 경우에 이벤트를 발생시키는 클래스를 변경하지 않고 재사용하고자 할 때 사용하는 디자인 패턴이다.

## 만능버튼 만들기 예시

### 기본 로봇 설계
![램프켜는 버튼을 설계한 클래스 다이어그램](https://user-images.githubusercontent.com/31653025/84618058-e9c93480-af0b-11ea-9bdf-d4e522ae0a35.PNG)

~~~java
package model;

public class Button {
	private Lamp lamp;
	
	public Button(Lamp lamp) {
		this.lamp = lamp;
	}
	
	public void pressed() {
		lamp.turnOn();
	}
}
~~~

### 문제점
* (문제 1) 누군가 버튼을 눌렀을 때 램프대신 다른 기능을 실행하게 하려면 어떤 변경 작업을 해야하는가? 예를 들어 알람이 켜지게 하려면?
* (문제 2) 버튼을 누르는 동작에 따라 다른 기능을 실행하게 하려면 어떤 변경 작업을 해야하는가? 예를 들어 버튼을 처음 눌렀을 때 램프를 켜고, 두 번째 눌렀을 때 알람을 동작하게 하려면?

### 문제 1 - 버튼을 눌렀을 때 다른 기능을 실행하는 경우
~~~java
package model;

public class Button {
	private Lamp lamp;
	private Alarm alarm;
	
	public Button(Alarm alarm) {
		// this.lamp = lamp;
		this.alarm = alarm;
	}
	
	public void pressed() {
		// lamp.turnOn();
		alarm.start();		
	}
}
~~~
* *Button클래스* 의 *pressed메서드* 를 수정해야한다. 즉 **OCP**에 위배된다.

### 문제 2 - 버튼을 누르는 동작에 따라 다른 기능을 실행하는 경우
~~~java
package model;

import enumeration.Mode;

public class Button {
	private Lamp lamp;
	private Alarm alarm;
	private Mode mode;
	
	public Button(Lamp lamp, Alarm alarm) {
		this.lamp = lamp;
		this.alarm = alarm;
	}
	
	public void setMode(Mode mode) {
		this.mode = mode;
	}
	
	public void pressed() {
		switch (mode) {
		case LAMP: // 램프모드인 경우
			lamp.turnOn();
			break;
		case ALARM: // 알람모드인 경우
			alarm.start();
			break;
		default:
			break;
		}		
	}
}
~~~
* *Button클래스* 의 *pressed메서드* 를 수정해야할 뿐만 아니라 새로운 기능 추가시 계속 수정해주어야한다.
* 극단적으로 모드의 갯수가 100개면 Button생성자에 파라미터 수도 100개가 될 것이고, pressed메서드의 case문도 100개가 될 것이다.

### 해결책
* *Button클래스* 의 *pressed메서드* 에서 직접 기능을 구현하지 않고 버튼을 눌렀을 때 *Button 클래스* 외부에서 제공받아 캡슐화하여 *pressed메서드* 에서 호출하도록 한다.

### 개선된 설계
![개선된 Button클래스 다이어그램](https://user-images.githubusercontent.com/31653025/84620646-83481480-af13-11ea-9549-9b2d2da74ef5.PNG)

~~~java
package model;

import command.Command;

public class Button {
	private Command command;
	
	public Button() {}
	
	public void setCommand(Command command) {
		this.command = command;
	}
	
	public void pressed() {
		command.execute();
	}
}
~~~
* 개선된 설계를 통해 *Button클래스* 의 *pressed()메서드* 는 command에게 실행을 위임하게 된다.
* 즉, Command가 추가되더라도 *pressed()메서드* 는 변경되지 않는다.

![일반적인 커맨드 패턴 클래스 다이어그램](https://user-images.githubusercontent.com/31653025/84621599-eaff5f00-af15-11ea-90ff-80dfd52028c2.PNG)

* Command : 실행될 기능에 대한 인터페이스. 실행될 기능을 execute메서드로 선언함
* ConcreteCommand : 실제로 실행되는 기능을 구현. 즉, Command 인터페이스를 구현함 (ex. AlarmStartCommand, LampOnCommand..)
* Invoker : 기능의 실행을 요청하는 호출자 클래스 (ex. Button..)
* Receiver : ConcreteCommand에서 execute메서드를 구현할 때 필요한 클래스. 즉, Concrete Command의 기능을 실행하기 위해 사용하는 수신자 클래스 (ex. Lamp, Alarm..)

## 참조
> - [JAVA 객체지향 디자인 패턴 정인상, 채흥석 저]