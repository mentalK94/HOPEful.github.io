---
layout: post
title: '[Design Pattern] 옵저버 패턴'
subtitle: 옵저버 메소드 패턴의 개념을 이해하고 구현할 수 있다.'
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

## 옵저버 패턴이란?

* 데이터의 변경이 발생했을 경우 상대 클래스나 객체에 의존하지 않고 데이터의 변경을 통보하고자 할 때 유용하다. 예를 들어 새로운 파일이 추가되거나 기존파일이 삭제되었을 때 탐색기는 이를 즉시 표시할 필요가 있다.

## 여러가지 방식으로 성적 출력하기 예시

### 기본 설계
![ScoreRecord 클래스의 값을 출력하는 DataSheetView 클래스 설계](https://user-images.githubusercontent.com/31653025/85218960-6bbcd000-b3da-11ea-9d7b-dc9bc67204f6.PNG)

```
package ex1;

import java.util.List;

public class DataSheetView {
	
	private int viewCount;
	private ScoreRecord scoreRecord;
	
	protected DataSheetView(int viewCount, ScoreRecord scoreRecord) {
		this.viewCount = viewCount;
		this.scoreRecord = scoreRecord;
	}

	protected void update() { // 점수의 변경을 통보받음
		List<Integer> record = scoreRecord.getScoreRecord();
		displayScores(record, viewCount);
	}
	
	private void displayScores(List<Integer> record, int viewCount) {
		System.out.print(viewCount + " entries : ");
		for(int i = 0; i<viewCount && i<record.size(); i++) {
			System.out.print(record.get(i) + " ");
		}
		System.out.println();
	}

}
```

```
package ex1;

import java.util.*;

public class ScoreRecord {
	private List<Integer> scores = new ArrayList<Integer>();
	private DataSheetView dataSheetView;
	
	protected void setDataSheetView(DataSheetView dataSheetView) {
		this.dataSheetView = dataSheetView;
	}
	
	protected void addScore(int score) { // 새로운 점수를 추가
		scores.add(score);
		dataSheetView.update();
	}
	
	protected List<Integer> getScoreRecord() {
		return scores;
	}
}
```

### 문제점
* (문제 1) 성적을 다른 형태로 출력하고 싶다면 어떤 변경 작업을 해야 하는가? 예를 들어 성적을 최대/최소 값만 출력하려면?
* (문제 2) 여러가지 형태의 성적을 동시 혹은 순차적으로 출력하려면 어떤 변경 작업을 해야 하는가? 예를 들어 성적을 모두 출력하고 최소/최대 값을 출력하려면?

### 문제 1 - 다른 형태로 출력하고 싶은 경우
```
package ex1;

import java.util.ArrayList;
import java.util.List;

public class ScoreRecord {
	private List<Integer> scores = new ArrayList<Integer>();
	// private DataSheetView dataSheetView;
	private MinMaxView minMaxView;
	
	protected void setDataSheetView(MinMaxView minMaxView) {
		this.minMaxView = minMaxView;
	}
	
	protected void addScore(int score) { // 새로운 점수를 추가
		scores.add(score);
		minMaxView.update();
	}
	
	protected List<Integer> getScoreRecord() {
		return scores;
	}
}
```
* *ScoreRecord클래스* 를 수정해야한다. 즉 **OCP**에 위배된다.

### 문제 2 - 동시 혹은 순차적으로 성적을 출력하는 경우

* *ScoreRecord클래스* 에 *DataSheetView클래스, MinMaxView클래스* 를 속성 값으로 추가하여 사용한다. -> *ScoreRecord클래스* 가 변경된다.
* 평균/표준편차를 출력하는 *StatisticView클래스* 가 추가되는 경우 *ScoreRecord클래스* 는 다시 변경되어야 한다.
* 결론적으로 *ScoreRecord클래스* 가 변경된다. ~~(*ScoreRecord클래스* 좀 가만 내버려둬...)~~ 즉 **OCP원칙**에 위배된다.

### 해결책
* 문제 해결의 핵심은 성적 통보 방법이 변경되더라도 *ScoreRecord클래스* 를 그대로 재사용할 수 있어야 한다는 점이다.
* *ScoreRecord클래스* 에서는 통보 대상인 **객체 참조하는 것을 관리**해야 하며 *addScore메서드* 는 각 통보 대상인 **객체의 *update메서드* 를 호출**할 필요가 있다.
* 이러한 기능을 **상위 클래스 및 인터페이스로 일반화**하고 이를 활용하여 *ScoreRecord를 구현* 하는 방식으로 설계를 변경해야 한다.

### 개선된 설계
![개선된 ScoreRecord의 클래스 다이어그램](https://user-images.githubusercontent.com/31653025/85219905-63689300-b3e2-11ea-94d2-a33aaaf489a7.PNG)
* *Subject라는 클래스* 를 정의하여 *attach메서드* 와 *detach메서드* 를 이용하여 성적 변경에 관심이 있는 객체를 추가하거나 제거한다.
* *ScoreRecord 클래스* 의 *addScore메서드* 가 호출되면 *Subject클래스* 의 *notifyObservers메서드* 를 호출한다.
* *notifyObservers메서드* 를 호출하여 옵저버들에게 성적 변경을 통보한다. 여기서 옵저버의 *update메서드* 를 호출한다.
* **생각해보라!** 여기서 *Observer인 Statistics클래스* 가 추가된다 하더라도 *ScoreRecord클래스* 는 영향을 받지 않는다.

```
package ex1;

import java.util.ArrayList;
import java.util.List;

public abstract class Subject {
	
	private List<Observer> observers = new ArrayList<Observer>();
	
	public void attach(Observer observer) {
		observers.add(observer);
	}
	
	public void detach(Observer observer) {
		observers.remove(observer);
	}
	
	public void notifyObservers() {
		for(Observer observer : observers) {
			observer.update();
		}
	}
}
```

```
package ex1;

import java.util.ArrayList;
import java.util.List;

public class ScoreRecord extends Subject {
	
	private List<Integer> scores = new ArrayList<Integer>();
	
	protected void addScore(int score) { // 새로운 점수를 추가
		scores.add(score);
		
		notifyObservers(); // 각 옵저버에게 데이터의 변경을 통보
	}
	
	protected List<Integer> getScoreRecord() {
		return scores;
	}
}
```

```
package ex1;

public interface Observer {
	public void update();
}
```

### 일반적인 옵저버 패턴
![일반적인 옵저버 패턴 다이어그램](https://user-images.githubusercontent.com/31653025/85220418-3cac5b80-b3e6-11ea-96b0-cad6ea8cccf8.PNG)

* Observer : 데이터의 변경을 통보 받는 인터페이스. 즉, Subject에서는 Observer인터페이스의 update메서드를 호출함으로써 ConcreteSubejct의 데이터 변경을 ConcreteObserver에게 통보한다.
* Subejct : ConcreteObserver 객체를 관리하는 요소. Observer 인터페이스를 참조해서 ConcreteObserver를 관리하므로 ConcreteObserver의 변화에 독립적일 수 있다.
* ConcreteSubejct : 변경 관리 대상이 되는 데이터가 있는 클래스. 데이터가 변경되는 메서드 호출 시 Subject의 notifyObservsers를 호출해서 ConcreteObserver 객체에 변경을 통보한다.
* ConcreteObserver : ConcreteSubject의 변경을 통보받는 클래스. Observer 인터페이스의 update메서드를 구현함으로써 변경을 통보받는다.

## 참조
> - [JAVA 객체지향 디자인 패턴 정인상, 채흥석 저]