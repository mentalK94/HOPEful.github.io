---
layout: post
title: '[Design Pattern] 데커레이터 패턴'
subtitle: 데커레이터 패턴의 개념을 이해하고 구현할 수 있다.'
date: 2020-06-21
author: hansol Kim
image: '/assets/article_images/designPattern/designPattern.jpg'
categories: DesignPattern
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 데커레이터 패턴이란?

* 데커레이터 패턴은 기본 기능에 추가할 수 있는 많은 종류의 부가 기능에서 파생되는 다양한 조합을 동적으로 구현할 수 있는 패턴이다.

## 도로 표시 방법 조합하기

### 기본 설계
![기본 도로 및 차선을 표시하는 클래스 설계](https://user-images.githubusercontent.com/31653025/85703738-9b414480-b71a-11ea-9f04-7742c0a0be1f.JPG)

```
package ex1;

public class RoadDisplay {
	public void draw() {
		System.out.println("기본 도로 표시");
	}
}
```

```
package ex1;

public class RoadDisplayWithLane extends RoadDisplay {
	
	public void draw() {
		super.draw();
		drawLane();
	}

	private void drawLane() {
		System.out.println("차선 표시");
	}
}
```

### 문제점
* (문제 1) 또다른 도로 표시 기능을 추가로 구현하고 싶다면 어떻게 해야 하는가? 예를 들어 기본 도로 표시에 교통량을 표시하고 싶다면?
* (문제 2) 여러가지 추가 기능을 조합해 제공하고 싶다면 어떻게 해야 하는가? 예를 들어 기본 도로 표시에 차선 표시 기능과 교통량 표시 기능을 함께 제공하고 싶다면?

### 문제 1 - 또다른 도로 표시 기능을 추가로 구현하는 경우
```
package ex1;

public class RoadDisplayWithTraffic extends RoadDisplay{
	
	public void draw() {
		super.draw();
		drawTraffic();
	}

	private void drawTraffic() {
		System.out.println("교통량 표시");
	}
}
```
* 기본 도로에 교통량을 표시하는 *RoadDisplayWithTraffic 클래스* 를 추가적으로 구현한 코드이다.
* 추가적으로 구현하는데에 있어 기존의 코드를 건드리지 않으므로 적절한 설계 방법이라고 볼 수 있다.

### 문제 2 - 여러 가지 추가 기능을 조합해야 하는 경우

* 다양한 기능의 조합을 고려해야 하는 경우 상속을 통한 기능의 확장은 각 기능별로 클래스를 추가해야 한다는 단점이 있다. (감이 잘 안올 수도 있는 부분이기에 아래 그림참고.)

![도로 표시 기능의 조합](https://user-images.githubusercontent.com/31653025/85703739-9bd9db00-b71a-11ea-9586-2ad6b4555ee8.JPG)

* 위와 같이 상속을 통해 조합을 할 경우 각 조합별로 클래스를 구현해야 한다. ~~(야근의 원인...ㅠ)~~
* 여기서 추가 기능이 하나씩 더 추가될 경우 조합의 수는 기하급수적으로 늘어나게 된다.

### 해결책
* 조합의 수가 늘어나는 문제를 해결할 수 있는 설계를 위해 추가 기능별로 개별적인 클래스를 설계한다.
* 기능을 조합할 때 각 클래스의 객체 조합을 이용한다.

### 개선된 설계
![개선된 추가 기능 조합의 설계](https://user-images.githubusercontent.com/31653025/85716943-ef9ef100-b727-11ea-9154-9c9b757be4d0.JPG)
* 도로를 표시하는 기본 기능만 사용할 경우 RoadDisplay객체를 이용한다.
* 추가 기능을 사용할 경우 기능에 맞는 객체를 이용한다.

```
public abstract class Display {
	public abstract void draw();
}
```

```
package ex1;

public abstract class DisplayDecorator extends Display{
	
	private Display decoratedDisplay;
	
	public DisplayDecorator(Display decoratedDisplay) {
		this.decoratedDisplay = decoratedDisplay;
	}
	
	@Override
	public void draw() {
		decoratedDisplay.draw();
	}
}
```

```
package ex1;

public class LaneDecorator extends DisplayDecorator {

	public LaneDecorator(Display decoratedDisplay) {
		super(decoratedDisplay);
	}
	
	@Override
	public void draw() {
		super.draw();
		drawLane();
	}
	
	private void drawLane() {
		System.out.println("차선 표시");
	}

}
```

```
package ex1;

public class TrafficDecorator extends DisplayDecorator {

	public TrafficDecorator(Display decoratedDisplay) {
		super(decoratedDisplay);
	}
	
	@Override
	public void draw() {
		super.draw();
		drawTraffic();
	}
	
	private void drawTraffic() {
		System.out.println("교통량 표시");
	}

}
```

### 개선된 설계에서 기능을 추가할 경우
![개선된 추가 기능 조합의 설계에서의 기능 추가](https://user-images.githubusercontent.com/31653025/85716946-f0378780-b727-11ea-8401-3ba439a42f8c.JPG)

```
package ex1;

public class CrossingDecorator extends DisplayDecorator {

	public CrossingDecorator(Display decoratedDisplay) {
		super(decoratedDisplay);
	}
	
	@Override
	public void draw() {
		super.draw();
		drawCrossing();
	}
	
	private void drawCrossing() {
		System.out.println("교차로 표시");
	}
}
```

* 기존의 코드를 건드리거나 조합에 따른 클래스 구현이 필요가 없다.
* 즉, 추가 기능에 대한 부분만 구현해주면 된다.

```
package ex1;

public class Client {

	public static void main(String[] args) {
		
		// 기본 도로
		Display roadDisplay = new RoadDisplay();
		//roadDisplay.draw();
		
		// 기본 도로 + 차선 표시
		Display roadWithLane = new LaneDecorator(roadDisplay);
		//roadWithLane.draw();
		
		// 기본 도로 + 교통량 표시
		Display roadWithTraffic = new TrafficDecorator(roadDisplay);
		//roadWithTraffic.draw();
		
		// 기본 도로 + 차선 표시 + 교차로 표시
		Display roadWithLaneAndCrossing = new CrossingDecorator(new LaneDecorator(roadDisplay));
		roadWithLaneAndCrossing.draw();
	}
```
* 위와 같이 원하는 조합에 따라 동적으로 사용할 수 있다.

### 일반적인 옵저버 패턴
![일반적인 데커레이터 패턴](https://user-images.githubusercontent.com/31653025/85718797-ced79b00-b729-11ea-860c-006f812e4dbb.JPG)

* Component : 기본 기능을 뜻하는 ConcreteComponent와 추가 기능을 뜻하는 Decorator의 공통 기능을 정의한다. 즉, 클라이언트는 Component를 통해 실제 객체를 사용한다.
* ConcreteComponent : 기본 기능을 구현하는 클래스다.
* Decorator : 많은 수가 존재하는 구체적인 Decorator의 공통 기능을 제공한다.
* ConcreteDecorator : Decorator의 하위 클래스로 기본 기능에 추가되는 개별적인 기능을 뜻한다.

## 참조
> - [JAVA 객체지향 디자인 패턴 정인상, 채흥석 저]