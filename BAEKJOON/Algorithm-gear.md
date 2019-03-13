---
title: "[Algorithm]백준-톱니바퀴"
date: 2019-02-15 18:20:00                             
tags: ["Algorithm","백준","톱니바퀴"]
categories: Algorithm                                    
---
# 톱니바퀴
>[출처](https://www.acmicpc.net/problem/14891)

## 문제 설명
총 8개의 톱니를 가지고 있는 톱니바퀴 4개가 아래 그림과 같이 일렬로 놓여져 있다. 또, 톱니는 N극 또는 S극 중 하나를 나타내고 있다. 톱니바퀴에는 번호가 매겨져 있는데, 가장 왼쪽 톱니바퀴가 1번, 그 오른쪽은 2번, 그 오른쪽은 3번, 가장 오른쪽 톱니바퀴는 4번이다.

![gear1](/images/gear1.png)

이때, 톱니바퀴를 총 K번 회전시키려고 한다. 톱니바퀴의 회전은 한 칸을 기준으로 한다. 회전은 시계 방향과 반시계 방향이 있고, 아래 그림과 같이 회전한다.

![gear2](/images/gear2.png)

![gear3](/images/gear3.png)



톱니바퀴를 회전시키려면, 회전시킬 톱니바퀴와 회전시킬 방향을 결정해야 한다. 톱니바퀴가 회전할 때, 서로 맞닿은 극에 따라서 옆에 있는 톱니바퀴를 회전시킬 수도 있고, 회전시키지 않을 수도 있다. 톱니바퀴 A를 회전할 때, 그 옆에 있는 톱니바퀴 B와 서로 맞닿은 톱니의 극이 다르다면, B는 A가 회전한 방향과 반대방향으로 회전하게 된다. 예를 들어, 아래와 같은 경우를 살펴보자.

![gear4](/images/gear4.png)

두 톱니바퀴의 맞닿은 부분은 초록색 점선으로 묶여있는 부분이다. 여기서, 3번 톱니바퀴를 반시계 방향으로 회전했다면, 4번 톱니바퀴는 시계 방향으로 회전하게 된다. 2번 톱니바퀴는 맞닿은 부분이 S극으로 서로 같기 때문에, 회전하지 않게 되고, 1번 톱니바퀴는 2번이 회전하지 않았기 때문에, 회전하지 않게 된다. 따라서, 아래 그림과 같은 모양을 만들게 된다.

![gear5](/images/gear5.png)

위와 같은 상태에서 1번 톱니바퀴를 시계 방향으로 회전시키면, 2번 톱니바퀴가 반시계 방향으로 회전하게 되고, 2번이 회전하기 때문에, 3번도 동시에 시계 방향으로 회전하게 된다. 4번은 3번이 회전하지만, 맞닿은 극이 같기 때문에 회전하지 않는다. 따라서, 아래와 같은 상태가 된다.

![gear6](/images/gear6.png)

톱니바퀴의 초기 상태와 톱니바퀴를 회전시킨 방법이 주어졌을 때, 최종 톱니바퀴의 상태를 구하는 프로그램을 작성하시오.
## 입력
첫째 줄에 1번 톱니바퀴의 상태, 둘째 줄에 2번 톱니바퀴의 상태, 셋째 줄에 3번 톱니바퀴의 상태, 넷째 줄에 4번 톱니바퀴의 상태가 주어진다. 상태는 8개의 정수로 이루어져 있고, 12시방향부터 시계방향 순서대로 주어진다. N극은 0, S극은 1로 나타나있다.

다섯째 줄에는 회전 횟수 K(1 ≤ K ≤ 100)가 주어진다. 다음 K개 줄에는 회전시킨 방법이 순서대로 주어진다. 각 방법은 두 개의 정수로 이루어져 있고, 첫 번째 정수는 회전시킨 톱니바퀴의 번호, 두 번째 정수는 방향이다. 방향이 1인 경우는 시계 방향이고, -1인 경우는 반시계 방향이다.
## 출력
총 K번 회전시킨 이후에 네 톱니바퀴의 점수의 합을 출력한다. 점수란 다음과 같이 계산한다.

- 1번 톱니바퀴의 12시방향이 N극이면 0점, S극이면 1점
- 2번 톱니바퀴의 12시방향이 N극이면 0점, S극이면 2점
- 3번 톱니바퀴의 12시방향이 N극이면 0점, S극이면 4점
- 4번 톱니바퀴의 12시방향이 N극이면 0점, S극이면 8점
## 입출력 예제
![gear7](/images/gear7.png)

## 소스 코드
**자바**
```java
package ag;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main { // Gear 클래스로 만들고 싶었지만 백준 체점을 위해 이름을  Main 클래스로 변경
    int[] state = null;
    public int top =0; // 톱니바퀴 처음 맨위의 인덱스
    public int right = 2; // 톱니바퀴 3시방향 처음 인덱스
    public int left = 6; // 톱니바퀴 9시방향 처음 인덱스
    public int direct = 0; // 톱니바퀴가 어떤 방향으로 돌지
    Main rightGear=null; // 톱니바퀴의 오른쪽 톱니바퀴
    Main leftGear=null; // 톱니바퀴의 왼쪽 톱니바퀴
    public Main(int[] state){ //톱니바퀴 클래스의 생성자로 톱니바퀴의 톱니에 적힌 극을 배열로 저장
        this.state=state;
    }
    public void setRightGear(Main rightGear){
        this.rightGear =rightGear;
    } // 오른쪽 톱니바퀴를 등록

    public void setLeftGear(Main leftGear){
        this.leftGear =leftGear;
    } // 왼쪽 톱니바퀴를 등록
    public void startTurn(int num){ // 맨처음 톱니바퀴를 돌릴때 실행시킬 메서드
        this.direct=num;
        if(rightGear!=null && leftGear!=null){ // 좌우 톱니바퀴가 모두 존재할 경우 양쪽으로 톱니바퀴 돌리는 메서드 실행
            rightTurn(direct*-1); //좌우 톱니바퀴는 자신과 반대로 돌기때문에 -1을 곱해 반대 값을 넘겨줌
            leftTurn(direct*-1);
        }
        else if(rightGear==null){ // 왼쪽 톱니바퀴만 존재할 경우
            leftTurn(direct*-1);
        }
        else if(leftGear==null){ // 오른쪽 톱니바퀴만 존재할 경우
            rightTurn(direct*-1);
        }
        turn(); //좌우 모든 톱니바퀴를 돌리고 나서 자신의 톱니바퀴를 돌림
    }
    public void turn(){ // 톱니바퀴를 돌리는 메서드 배열을 직접 바꾸지않고 top, left, right 값만 바꿔줌
        if(direct ==-1){
            if(top==7)top =0;
            else top++;
            if(right==7)right=0;
            else right++;
            if(left==7)left=0;
            else left++;
        }
        else if(direct==1){
            if(top==0)top =7;
            else top--;
            if(right==0)right=7;
            else right--;
            if(left==0)left=7;
            else left--;
        }

    }
    public void rightTurn(int num){
        rightGear.direct=num; // 오른쪽 톱니바퀴의 방향에 자신과 반대의 값을 넣어줌
        if(rightGear.rightGear==null&&this.state[this.right]!=rightGear.state[rightGear.left]){ //오른쪽 톱니바퀴의 오른쪽 톱니바퀴가 존재하지않고 자신의 오른쪽 값과 오른쪽 톱니바퀴의 왼쪽 값이 다를경우
            rightGear.turn(); //오른쪽 톱니바퀴만 돌림
            return;
        }
        else if(this.state[this.right]!=rightGear.state[rightGear.left]){ //오른쪽 톱니바퀴의 오른쪽 톱니바퀴가 존재하고 자신의 오른쪽 값과 오른쪽 톱니바퀴의 왼쪽 값이 다를경우
            rightGear.rightTurn(rightGear.direct*-1); //오른쪽 톱니바퀴의 rightTurn 메서드를 실행시켜 오른쪽 톱니바퀴의 오른쪽 톱니바퀴도 값비교후 돌림
            rightGear.turn();
            return;
        }
        else{ // 자신의 오른쪽 값과 오른쪽 톱니바퀴의 왼쪽값이 다를 경우는 아무 작업 하지 않고 리턴
            return;
        }
    }
    public void leftTurn(int num){
        leftGear.direct=num;
        if(leftGear.leftGear==null && this.state[this.left]!=leftGear.state[leftGear.right]){
            leftGear.turn();
            return;
        }
        else if(this.state[this.left]!=leftGear.state[leftGear.right]){
            leftGear.leftTurn(leftGear.direct*-1);
            leftGear.turn();
            return;
        }
        else{
            return;
        }
    }

    public int score(){ //맨위의 값이 1일 경우는 점수있음
        if(this.state[this.top]==1)return 1;
        else return 0;
    }
    static BufferedReader br =null;
    public static void main(String[] args) throws IOException {
        int answerScore =0; //정답을 만들기 위한 변수
        List<int[]> arrayList = new ArrayList<>();

        br = new BufferedReader(new InputStreamReader(System.in));
        for(int i=0; i<4;i++){
            int[] gearStateArray =new int[8];
            String arrayString = br.readLine();
            for(int j=0;j<arrayString.length();j++){
                gearStateArray[j]=arrayString.charAt(j) -'0';
            }
            arrayList.add(gearStateArray);
        }

        Main gear1 = new Main(arrayList.get(0)); // 톱니바퀴 1 생성
        Main gear2 = new Main(arrayList.get(1)); // 톱니바퀴 2 생성
        Main gear3 = new Main(arrayList.get(2)); // 톱니바퀴 3 생성
        Main gear4 = new Main(arrayList.get(3)); // 톱니바퀴 4 생성

        gear1.setRightGear(gear2); //톱니바퀴의 양옆 톱니바퀴들 등록
        gear2.setLeftGear(gear1); //톱니바퀴의 양옆 톱니바퀴들 등록
        gear2.setRightGear(gear3); //톱니바퀴의 양옆 톱니바퀴들 등록
        gear3.setLeftGear(gear2); //톱니바퀴의 양옆 톱니바퀴들 등록
        gear3.setRightGear(gear4); //톱니바퀴의 양옆 톱니바퀴들 등록
        gear4.setLeftGear(gear3); //톱니바퀴의 양옆 톱니바퀴들 등록

        int count = Integer.parseInt(br.readLine());
        int gear,direct;
        for(int i=0; i<count;i++){
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            gear =Integer.parseInt(st.nextToken());
            direct = Integer.parseInt(st.nextToken());
            if(gear==1 && direct==1) gear1.startTurn(1);
            else if(gear==1 && direct==-1) gear1.startTurn(-1);
            else if(gear==2 && direct==1) gear2.startTurn(1);
            else if(gear==2 && direct==-1) gear2.startTurn(-1);
            else if(gear==3 && direct==1) gear3.startTurn(1);
            else if(gear==3 && direct==-1) gear3.startTurn(-1);
            else if(gear==4 && direct==1) gear4.startTurn(1);
            else if(gear==4 && direct==-1) gear4.startTurn(-1);
        }

        answerScore+=gear1.score() + (gear2.score()*2) + (gear3.score()*4) + (gear4.score()*8); //각 톱니바퀴에 점수에 맞게 계산


        System.out.println(answerScore);

    }
}

```
## 소스코드 설명

저는 우선 링크드 리스트를 이용하여 톱니바퀴가 바뀔때마다 리스트를 변경하려고 하였습니다. 

그러나 그냥 좌, 우, 위의 인덱스 정보만 저장 해놓고 변경하는식으로 사용하면 될 거같아 배열을 사용하기로 하였습니다.

톱니바퀴 객체를 만들고 그안에 인덱스와 배열 정보들을 기초변수로 각 기어의 양 옆 기어들을 저장시키기위한 톱니바퀴 클래스 참조변수 두개를 변수로 두고 set 메서드로 연결 시키도록 하였습니다.

turn 메서드는 톱니바퀴가 돌았을때의 효과를 주기 위한 인덱스 변경 메서드입니다.

startTurn 메서드는 맨 처음 입력값에서 톱니바퀴를 돌릴 경우 양 옆의 톱니바퀴가 존재할 경우 양 옆으로 회전을 전이시키기 위해 따로 작성한 메서드 입니다.

leftTurn, rightTurn 메서드는 각각 옆의 톱니바퀴가 돌아야 한다면 돌리고, 옆의 톱니바퀴에서도 메서드를 실행시켜 옆옆의 톱니바퀴들도 게속 확인하며 돌리는 메서드입니다.
