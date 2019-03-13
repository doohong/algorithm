---
title: "[Algorithm]프로그래머스-정수 삼각형"
date: 2019-03-13 15:20:00                             
tags: ["Algorithm","프로그래머스","동적계획법","정수삼각형"]
categories: Algorithm                                    
---
# 정수 삼각형

## 문제 설명

![triangle](/images/triangle.png)

위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.
## 제한사항

삼각형의 높이는 1 이상 500 이하입니다.

삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

## 입출력 예
|triangle|return|
|---|---|
|[[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]]|30|


[출처](http://stats.ioinformatics.org/countries/SWE)

## 소스 코드
**자바**
```java
class Solution {
    int answer=0;
    public int solution(int[][] triangle) {
        
        for(int i =triangle.length-2;i>=0;i--){
            for(int j=0;j<triangle[i].length;j++){
                triangle[i][j]=triangle[i+1][j]>triangle[i+1][j+1]?triangle[i][j]+triangle[i+1][j]: triangle[i][j]+ triangle[i+1][j+1];
            }
        }
        return triangle[0][0];
    }  
}
```
## 소스코드 설명

처음에는 재귀함수를 통하여 모든 결과를 더하고 맨마지막 행에 도달했을 때 점수가 최대값보다 크면 최대값을 갱신하는 방법을 이용하였습니다.

문제가 원하는 방법이 아니듯 시간 초과로 실패 하였고 다른 방법을 생각하였습니다.

갈수 있는 길은 [i][j]일 경우 [i+1][j] or [i+1][j+1]이기 때문에 반대로 올라가는 방법을 생각하게 되었고

[i][j]에 [i+1][j] 과 [i+1][j+1] 더 큰값을 더해주면서 삼각형 맨 위까지 반복문을 돌리게 되면 triangle[0][0]에는 최대 값이 저장 됩니다.
  