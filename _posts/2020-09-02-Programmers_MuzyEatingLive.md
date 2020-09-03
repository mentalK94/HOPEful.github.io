---
layout: post
title: '[2019 카카오 블라인드 채용] 무지의 먹방 라이브 리뷰'
subtitle: 
date: 2020-09-02
author: hansol Kim
image: '/assets/article_images/algorithm/algorithm.jpg'
categories: ProblemSolving
tags: 
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

### 문제 링크
* [2019 kakao blind recruitment - 무지의 먹방 라이브](https://programmers.co.kr/learn/courses/30/lessons/42891)


### 환경
* Java JDK 1.8


### 문제 이슈
* 특정 알고리즘을 사용하는 문제는 아니고 단순 시뮬레이션 문제인데 효율성을 상당히 고민해야하는 문제
* 효율성 테스트에서 k는 최대 **2x10^13** 이므로 **1초**씩 줄여나가는 로직으로는 이 문제를 풀 수 없다. (k를 어떻게 줄여나갈 수 있을 것인가?)
* 효율성 테스트에서 **food_time** 의 원소값의 최대 크기가 **100,000,000** 이므로 **food_time** 의 길이가 **200,000** 인 것을 고려하면 로직에서 food_time원소 값의 관련된 합 연산시  **int** 형으로 불가능하다.


### 접근방법
* 1) 가장 짧게 걸리는 음식이 무엇인지 알아야하기 때문에 주어진 음식배열을 깊은복사로 가져온 후 오름차순으로 정렬한 배열을 만든다.
* 2) 먹어야 할 음식들 중 가장 짧게 걸리는 음식을 기준으로(t1초) 전체 음식을 t1초동안 먹을 수 있는 시간이 충분한 지 확인
* 2-1-1) 충분하다면 전체음식을 t1초간 먹었다고 가정하고 전체 배열을 t1의 시간만큼 빼준다. 그리고 k는 **(t1 * 먹을 수 있는 음식의 수)** 만큼 빼준다.
* 2-1-2) 음식을 다 먹은 것은 앞으로 음식을 먹을 때 시간이 필요 없기 때문에 **먹을 수 있는 음식의 수** 에서 -1을 연산한다.
* 2-1-3) 가장 짧게 걸리는 음식을 관리하는 배열에서 가장 짧게 걸리는 음식을 다 먹었으니 **인덱스값** +1을 연산한다.
* 2-1-4) 만약 남은 음식의 수가 여러 개일 수도 있으므로 확인한 후 **먹을 수 있는 음식의 수**와 **인덱스값** 추가적으로 조정한다.
* 3) 2)의 과정을 반복 후 **먹을 수 있는 음식의 수** 가 없을 때 -1을 반환하도록 한다.
* 2-2) 충분하지 않다면 k값이 현재 남은 음식길이보다 짧게 만들어준 후에 선형탐색을 통해 다음에 먹어야 할 음식을 찾는다.


### 구현
~~~java
/**
 * programmers - 2019 카카오 블라인드 채용. 무지의 먹방 라이브
 * MuzyEatingLive.java
 * @date 2020-08-25
 * @author hansolKim
 **/
import java.util.Arrays;

public class Solution {

	public int solution(int[] food_times, long k) {
		int answer = 0;
		int foodSize = food_times.length;
		int minIdx = 0;
		int[] sortTimes = food_times.clone();

		Arrays.sort(sortTimes);

		int min = 0;

		main: while (true) {

			if (foodSize == 0) {
				answer = -1;
				break;
			}

			int minValue = sortTimes[minIdx] - min;

			/* (음식들을 먹는 시간들 중 최소값*음식의 수)과 k와 비교 */
			if (k >= ((long)minValue * foodSize)) { // 최소값보다 k가 큰 경우 ---> 모든 배열의 값을 최소값만큼 빼준다.
				for (int i = 0; i < food_times.length; i++) {
					food_times[i] -= minValue;
				}
				
				k -= (minValue * foodSize);
				foodSize--;
				min += minValue;
				minIdx++;

				/* 최소값이 되는 것이 여러개인 경우 ---> 0값이 되는 음식이 여러개인 경우인지 확인 ---> minIdx, foodSize 조절 */
				for (int i = minIdx-1; i < sortTimes.length - 1; i++) {
					if (sortTimes[i] != sortTimes[i + 1]) {
						break;
					}
					minIdx++;
					foodSize--;
				}
			} else {
				long idx = (k>=foodSize) ? (int) (k % foodSize) : k;
				for (int i = 0; i < food_times.length; i++) {
					if (food_times[i] > 0) {
						if (idx == 0) {
							answer = i + 1;
							break main;
						}
						idx--;
					}
				}
			}
		}
		
		return answer;
	}
}
~~~


### 문제
* [문제 해설](https://tech.kakao.com/2018/09/21/kakao-blind-recruitment-for2019-round-1/)