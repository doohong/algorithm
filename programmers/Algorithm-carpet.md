---
title: "[Algorithm]프로그래머스-카펫"
date: 2019-02-02 18:20:00                             
tags: ["Algorithm","프로그래머스","완전탐색","카펫"]
categories: Algorithm                                    
---
# 카펫

## 문제 설명
Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 빨간색으로 칠해져 있고 모서리는 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

![carpet](/images/carpet.png)

Leo는 집으로 돌아와서 아까 본 카펫의 빨간색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 빨간색 격자의 수 red가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.
## 제한사항
- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 빨간색 격자의 수 red는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

## 입출력 예
|brown|red|return|
|---|---|---|
|10|2|[4,3]|
|8|1|[3,3]|
|24|24|[8,6]|
[출처](http://hsin.hr/coci/archive/2010_2011/contest4_tasks.pdf)
## 소스 코드
**자바**
```java
class Solution {
    public int[] solution(int brown, int red) {
        int[] answer = new int[2];
        int garo;
        int sero;
        for(int i=1; i<= red;i++){
            if(red%i==0){
                garo = i >= red/i? i :red/i;
                sero = i >= red/i? red/i :i;
                System.out.println(garo);
                System.out.println(sero);
                if(brown == (sero*2)+((garo+2)*2)){
                    answer[0]=garo+2;
                    answer[1]=sero+2;
                    break;
                }
            }
        }
        return answer;
    }
}
```
## 소스코드 설명

저는 카펫에 갈색 부분이 빨간 부분의 세로*2 개, (가로+2)*2 개만큼 있다는걸 발견 하였습니다.

빨간 부분의 개수를 이용해서 가로 세로가 되는 경우를 전부 구하며 그경우에 위에 식을 대입하여 갈색 부분의 갯수와 일치 하는지 확인하였습니다.

일치하는 경우가 빨간 부분의 가로 세로가 되고 전체 카펫의 가로와 세로의 길이는 빨간 부분의 가로와 세로에 각각 2개만큼 더한게 됩니다. 

몫과 나머지를 이용하여 빨간 부분의 가로세로를 구하는 부분에서 더 큰값을 가로로 잡기위해 삼항연산자를 사용하였습니다.
