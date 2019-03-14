---
title: "[Algorithm]프로그래머스-도둑질"
date: 2019-03-14 15:20:00                             
tags: ["Algorithm","프로그래머스","동적계획법","도둑질"]
categories: Algorithm                                    
---

# 도둑질

## 문제 설명
도둑이 어느 마을을 털 계획을 하고 있습니다. 이 마을의 모든 집들은 아래 그림과 같이 동그랗게 배치되어 있습니다.

![thievery](/images/thievery.png)

각 집들은 서로 인접한 집들과 방범장치가 연결되어 있기 때문에 인접한 두 집을 털면 경보가 울립니다.

각 집에 있는 돈이 담긴 배열 money가 주어질 때, 도둑이 훔칠 수 있는 돈의 최댓값을 return 하도록 solution 함수를 작성하세요.

## 제한사항
이 마을에 있는 집은 3개 이상 1,000,000개 이하입니다.

money 배열의 각 원소는 0 이상 1,000 이하인 정수입니다.

## 입출력 예
|money|return|
|---|---|
|[1,2,3,1]|4|


## 소스 코드
**자바**
```java
class Solution {
    public int solution(int[] money) {
        int answer = 0;
        int length = money.length;
        int[] dp =new int[length-1];
        int[] dp2= new int[length];
        
        dp[0]=money[0];
        dp[1]=money[0];
        dp2[0]=0;
        dp2[1]=money[1];
        for(int i=2;i<length-1;i++){
            dp[i]=Math.max(dp[i-2]+money[i],dp[i-1]);
        }
        for(int i=2;i<length;i++){
            dp2[i]=Math.max(dp2[i-2]+money[i],dp2[i-1]);
        }
        
        return Math.max(dp[length-2],dp2[length-1]);
    }
}
```
## 소스코드 설명

값을 저장할 dp배열을 2개를 만들었습니다. 한개는 처음 집을 훔치는 경우이며 다른 한개는 처음 집을 훔치지 않는 경우 입니다.

dp배열은 처음 집을 훔치기때문에 인접한 마지막 집은 훔칠수 없으므로 반복문은 length-1 전까지만 돌렸습니다.

dp[0]과 dp[1]은 0번째 집부터 1번째 집까지 가장 많이 훔칠수 있는 금액인데 0번집을 훔치기때문에 1번째 집은 훔칠수없게되고 dp[1]까지의 가장큰 금액은 첫번째 집을 훔친 경우이므로 dp[0],dp[1]에는 0번 집의 돈을 넣어 줬습니다.

반복문을 돌면서 두번째 전의 최대 돈에 현재 번째 집의 돈을 합친것과 이전의 최대 돈을 비교하여 dp배열을 채웁니다.

dp2의 경우는 0번째 집에서는 돈을 훔치지 않는 경우이며 0을 넣어주고 dp2[1]에는 1번째 집의 돈을 넣어주고 똑같은 방법으로 반복을 시킵니다. 

dp2는 첫번째 집에서는 돈을 훔치지 않으므로 반복문은 length까지 돌렸습니다.

dp와 dp2의 마지막 값을 비교하여 더 큰값을 출력 하여 정답을 구하였습니다.
 