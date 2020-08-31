---
layout: post
title: '[Algorithm] 조합(Combination) 코드 구현'
subtitle: 조합의 개념을 이해하고 구현할 수 있다.'
date: 2020-08-31
author: hansol Kim
image: '/assets/article_images/algorithm/algorithm.jpg'
categories: Algorithm
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 조합이란?
* 서로 다른 *n*개의 원소에서 *r*개를 **중복없이** 골라 **순서에 상관없이** 선택하는 것을 **조합** 이라한다.

## [Algorithm] 재귀함수를 통해 순열 조합 구현하기

### 환경
* Java


### nPr 구현
~~~java
private static int N, R;
private static int[] numbers, input;

private static void combination(int cnt, int start) {
  if(cnt == R) { // (1)
    // System.out.println(Arrays.toString(numbers));
    return;
  }
  
  for(int i=start; i<N; i++) {
    numbers[cnt] = input[i];
    combination(cnt+1, i+1);
  }
}
~~~

* combination을 구하기 위해서는 nCr에서 n과 r값이 있어야 하므로 N, R을 int형으로 선언해놓는다.
* 여기서는 조합에 따르는 데이터를 input배열(int형 배열)을 준비했는데 상황에 맞게 사용하면 된다. (ex. 사과, 바나나, 포도의 순열을 정하고 싶다면 String배열을 사용해도 무방)
* 선택된 데이터의 값 즉, 순열을 만들기 위해서 필요한 배열인 numbers배열을 준비한다. 위와 같이 data가 String형이면 String배열로 준비하면 된다.
* combination을 재귀적으로 구현했으며, 파라미터 cnt값이 0부터 출발해서 선택할 때마다 +1을 한 후 재귀호출을 통해 R개 만큼 순열을 만들게 되면 하나의 순열이 만들어진다. 위의 코드에서 (1)에서 대부분의 로직을 처리하게 된다.
* 앞에서의 순열로직과 비교해보면 좀 더 간단한 것을 알 수 있다.


## 참조
> - [위키백과](https://ko.wikipedia.org/wiki/%EC%A1%B0%ED%95%A9)
> - [나무위키](https://namu.wiki/w/%EC%A1%B0%ED%95%A9)