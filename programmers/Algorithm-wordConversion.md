---
title: "[Algorithm]프로그래머스-단어 변환"
date: 2019-03-15 15:20:00                             
tags: ["Algorithm","프로그래머스","너비우선탐색","BFS","단어 변환"]
categories: Algorithm                                    
---
# 단어 변환

## 문제 설명

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

>1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
>2. words에 있는 단어로만 변환할 수 있습니다.
예를 들어 begin이 hit, target가 cog, words가 [hot,dot,dog,lot,log,cog]라면 hit -> hot -> dot -> dog -> cog와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.
## 소스 코드
**자바**
```java
import java.util.*;
class Solution {
    public int solution(String begin, String target, String[] words) {
      
        int[] countarray = new int[words.length]; //words배열 안의 단어까지 도달하는 카운트를 저장하는 배열
        boolean[][] array = new boolean[words.length][words.length]; //각 단어에서 단어로 변환 될수 있는지를 나타내기 위한 배열
        int targetindex=-1; //최종으로 변환되어야 하는 단어의 인덱스를 저장하기 위한 변수
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i=0; i<words.length;i++){ //시작 단어에서 변환 가능한 단어들을 찾기위한 반복문
            int count =0;
            for(int j=0;j<begin.length();j++){
                if(count==2)break;
                if(words[i].charAt(j)!=begin.charAt(j)){
                    count++;
                }
            }
            if(count==1){ //변환이 가능하면 큐에 집어 넣고 도달하는 카운트를 1로 저장
                queue.offer(i); 
                countarray[i]++;
            }
            if(words[i].equals(target)){ //단어가 최종 단어이면 인덱스를 저장
                targetindex=i;
            }
        }
        if(targetindex==-1) return 0; //배열안에 최종 단어가 존재하지 않으면 0을 반환
        for(int i=0; i<words.length;i++){  //단어에서 다른 단어로 변환이 가능 하는지 확인하는 반복문 
            for(int j=i+1;j<words.length;j++){
                int count =0;
                for(int k=0;k<begin.length();k++){
                    if(count==2)break;
                    if(words[i].charAt(k)!=words[j].charAt(k))count++;
                }
                if(count==1){ //i단어에서 j단어로 변환이 가능하면 array[i][j]를 true로 
                    array[i][j]=true;
                    array[j][i]=true;//방향성이 없기때문에 array[j][i]도 true로
                }
            }
        }
        while(!queue.isEmpty()){ //BFS시작
            int index = queue.poll();
            for(int i=0; i<words.length;i++){
                if(countarray[i]==0 &&array[index][i]){
                    countarray[i]=countarray[index]+1;
                    queue.offer(i);
                   
                }
            }
        }
        return countarray[targetindex];
    }
}
```
## 소스코드 설명

단어간 변화 할수 있는지 charAt함수를 이용하여 한글자씩 비교 하였고 한개만 다른 경우에는 변화 할 수 있다 생각하여 배열에 양방향으로 true를 주었습니다.

배열을 만들고 큐를 이용하여 반복문을 돌려 BFS를 구현하였습니다. 이미 탐색한 문자열은 countarray를 이용하여 체크하며 해당 문자열이 몇번째로 탐색 되었는지 카운트를 입력하였으며 

countarryay[targetindex]를 이용하여 목표 문자열은 몇번째로 탐색 되었는지 결과값을 리턴하였습니다.