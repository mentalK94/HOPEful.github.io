---
layout: post
title: '[SW Expert] 하나로'
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
* [[S/W 문제해결 응용] 4일차 - 하나로](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15StKqAQkCFAYD)


### 환경
* Java JDK 1.8


### 문제 간단 설명
* 인도네시아 내의 모든 섬들을 연결해야 하는데 환경 부담 세율과 해저터널 길이가 주어져 최소만큼 지불하여 만드는 문제이다.
* N개의 정점이 주어지면 N-1개의 간선으로 모든 정점을 연결하고 이렇게 연결된 비용이 가장 최소가 되도록 한다.
* 비용 계산은 환경 부담 세율(E) 각 해저터널 길이(L)의 제곱의 곱 즉, E * L^2 값이다.


### 문제 이슈
* 


### 접근방법



### 구현
~~~java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Solution {
	
	private static StringBuilder sb;
	private static long answer;
	private static int N;
	private static ArrayList<int[]> list;
	private static long[] dist;
	private static boolean[] visited;
	private static final long INF = Long.MAX_VALUE;
	
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		sb = new StringBuilder();
		
		int T = Integer.parseInt(br.readLine()); // 테스트 케이스 수 입력

		for (int test_case = 1; test_case <= T; test_case++) {

			sb.append("#").append(test_case).append(" ");
			
			N = Integer.parseInt(br.readLine()); // 좌표 수 입력
			list = new ArrayList<>();
			dist = new long[N];
			visited = new boolean[N];
			answer = 0;
			
			StringTokenizer st1 = new StringTokenizer(br.readLine());
			StringTokenizer st2 = new StringTokenizer(br.readLine());
			double E = Double.parseDouble(br.readLine());
			
			for(int i=0; i<N; i++) {
				list.add(new int[] {Integer.parseInt(st1.nextToken()), Integer.parseInt(st2.nextToken())});
			}
			
			Arrays.fill(dist, INF);
			dist[0] = 0 ;
			dijkstra();
			
			for(int i=0; i<N; i++) {
				answer += dist[i];
			}
			
			answer = Math.round(answer*E);
			sb.append(answer).append("\n");
		}
		System.out.println(sb);
	}

	private static void dijkstra() {
		
		for(int i=0; i<N; i++) {
			long minDist = INF; 
			int currentIdx = 0;
			
			// 1. 가장 짧은 비용이 드는 정점을 선택			
			for(int j=0; j<N; j++) {
				if(!visited[j] && dist[j] < minDist) {
					minDist = dist[j];
					currentIdx = j;
				}
			}

			// 2. 해당 정점 방문표기
			visited[currentIdx] = true;
			
			// 3. 해당 정점에서 방문하지 않은 정점에서 dist가 짧은 경우 dist 갱신
			for(int j=0; j<list.size(); j++) {
				int startX = list.get(currentIdx)[0];
				int startY = list.get(currentIdx)[1];
				int destX = list.get(j)[0];
				int destY = list.get(j)[1];
				
				long distValue = getDist(startX, startY, destX, destY);
				if(!visited[j] && distValue < dist[j]) {
					dist[j] = distValue;
				}
			}
		}
		
	}
	
	private static long getDist(int startX, int startY, int destX, int destY) {
		int x = Math.abs(startX-destX);
		int y = Math.abs(startY-destY);
		return (long)x*x + (long)y*y;
	}

}
~~~