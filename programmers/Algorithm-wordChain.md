---
title: "[Algorithm]프로그래머스-영어 끝말잇기"
date: 2019-02-03 18:20:00                             
tags: ["Algorithm","프로그래머스","끝말잇기","영어 끝말잇기"]
categories: Algorithm                                    
---
# 영어 끝말잇기

## 문제 설명
1부터 n까지 번호가 붙어있는 n명의 사람이 영어 끝말잇기를 하고 있습니다. 영어 끝말잇기는 다음과 같은 규칙으로 진행됩니다.

1. 1번부터 번호 순서대로 한 사람씩 차례대로 단어를 말합니다.
2. 마지막 사람이 단어를 말한 다음에는 다시 1번부터 시작합니다.
3. 앞사람이 말한 단어의 마지막 문자로 시작하는 단어를 말해야 합니다.
4. 이전에 등장했던 단어는 사용할 수 없습니다.
5. 한 글자인 단어는 인정되지 않습니다.

다음은 3명이 끝말잇기를 하는 상황을 나타냅니다.

tank → kick → know → wheel → land → dream → mother → robot → tank

위 끝말잇기는 다음과 같이 진행됩니다.

- 1번 사람이 자신의 첫 번째 차례에 tank를 말합니다.
- 2번 사람이 자신의 첫 번째 차례에 kick을 말합니다.
- 3번 사람이 자신의 첫 번째 차례에 know를 말합니다.
- 1번 사람이 자신의 두 번째 차례에 wheel을 말합니다.
- (계속 진행)

끝말잇기를 계속 진행해 나가다 보면, 3번 사람이 자신의 세 번째 차례에 말한 tank 라는 단어는 이전에 등장했던 단어이므로 탈락하게 됩니다.

## 제한 사항
- 끝말잇기에 참여하는 사람의 수 n은 2 이상 10 이하의 자연수입니다.
- words는 끝말잇기에 사용한 단어들이 순서대로 들어있는 배열이며, 길이는 n 이상 100 이하입니다.
- 단어의 길이는 2 이상 50 이하입니다.
- 모든 단어는 알파벳 소문자로만 이루어져 있습니다.
- 끝말잇기에 사용되는 단어의 뜻(의미)은 신경 쓰지 않으셔도 됩니다.
- 정답은 [ 번호, 차례 ] 형태로 return 해주세요.
- 만약 주어진 단어들로 탈락자가 생기지 않는다면, [0, 0]을 return 해주세요.

## 입출력 예
|n|words|result|
|---|---|---|
|3|[tank, kick, know, wheel, land, dream, mother, robot, tank]|[3,3]|
|5|[hello, observe, effect, take, either, recognize, encourage, ensure, establish, hang, gather, refer, reference, estimate, executive]|[0,0]|
|24|[hello, one, even, never, now, world, draw]|[1,3]|
## 소스 코드
**자바**
```java
import java.util.HashSet;
class Solution {
    public int[] solution(int n, String[] words) {
        int[] answer = {0,0};
        char wordStart; // 두번째 단어부터 첫번째 문자 저장을 위한 변수 선언
        char wordEnd = words[0].charAt(words[0].length()-1); //처음 단어의 마지막 문자를 저장
        HashSet<String> hashSet = new HashSet<String>(); 
        hashSet.add(words[0]); // 첫단어를 해쉬셋에 저장
        for(int i=1; i<words.length;i++){ 
            hashSet.add(words[i]);
            wordStart = words[i].charAt(0);
            if(wordEnd!=wordStart || hashSet.size() != i+1){
                answer[0] = (i%n)+1;
                answer[1] = (i/n)+1;
                break;
            }
            wordEnd= words[i].charAt(words[i].length()-1); //다음단어의 처음 알파벳 비교를 위해 이번 단어의 마지막 알파벳 저장
        }
        return answer;
    }
}
```
## 소스코드 설명

1. 단어의 끝 알파벳과 그 다음 첫 알파벳이 다른 단어를 찾아라.
2. 중복된 단어를 찾아라.

문자열이 저장된 배열에서 단어 한개씩 새로운 자료구조에 담으면서 문자열의 마지막 문자와 다음 문자의 처음 문자를 비교하면서 1번을 해결하려 하였습니다.

담으면서 중복됬는지를 확인하기 위하여 자료구조는 HashSet을 이용하였습니다. 

HashSet에는 중복 문자가 한번밖에 저장되지 않는 점을 이용하여 size()메서드를 이용하여 담기는 문자열이 이미 사용된 문자열인지 판단하였습니다. 
 