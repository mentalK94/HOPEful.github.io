---
layout: post
title: '[Algorithm] 순열(Permutation) 코드 구현'
subtitle: 순열의 개념을 이해하고 구현할 수 있다.'
date: 2020-07-28
author: hansol Kim
image: '/assets/article_images/algorithm/algorithm.jpg'
categories: Algorithm
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 순열이란?
* 서로 다른 *n*개의 원소에서 *r*개를 **중복없이** 골라 **순서에 상관있게** 나열하는 것을 *n*개에서 *r*개를 택하는 **순열** 이라한다.

## [Algorithm] 재귀함수를 통해 순열 코드 구현하기

### 환경
* Java

### nPr 구현
~~~java
private static int N, R;
private static int[] numbers, input;
private static boolean[] isSelected;

private static void permutation(int cnt) {
  if(cnt == R) { // (1)
    // System.out.println(Arrays.toString(numbers));
    return;
  }
  
  for(int i=0; i<N; i++) {
    if(isSelected[i]) { continue; }
    
    isSelected[i] = true;
    numbers[cnt] = input[i];
    permutation(cnt+1); // (2)
    isSelected[i] = false; // (3)
  }
}
~~~

* permutation을 구하기 위해서는 nPr에서 n과 r값이 있어야 하므로 N, R을 int형으로 선언해놓는다.
* 여기서는 순열에 따르는 데이터를 input배열(int형 배열)을 준비했는데 상황에 맞게 사용하면 된다. (ex. 사과, 바나나, 포도의 순열을 정하고 싶다면 String배열을 사용해도 무방)
* 선택된 데이터의 값 즉, 순열을 만들기 위해서 필요한 배열인 numbers배열을 준비한다. 위와 같이 data가 String형이면 String배열로 준비하면 된다.
* 선택되었을 때 구분하기 위해서 선택여부를 판단하는 isSelected배열을 준비한다.
* permutation을 재귀적으로 구현했으며, 파라미터 cnt값이 0부터 출발해서 선택할 때마다 +1을 한 후 재귀호출을 통해 R개 만큼 순열을 만들게 되면 하나의 순열이 만들어진다. 위의 코드에서 (1)에서 대부분의 로직을 처리하게 된다.
* 특히 (2)부분에서 현재 인덱스 i번째 데이터를 선택한 후 cnt+1을 하여 재귀호출을 하지만 (3)의 경우는 선택하지 않고 for loop의 다음을 탐색하는 경우를 위해 i번째 선택여부를 false로 바꾸어 준다.


## 참조
> - [위키백과](https://ko.wikipedia.org/wiki/%EC%88%9C%EC%97%B4)
> - [나무위키](https://namu.wiki/w/%EC%88%9C%EC%97%B4)