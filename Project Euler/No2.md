# Project Euler NO.2

-------------------------
피보나치 수열의 각 항은 바로 앞의 항 두 개를 더한 것이 됩니다. 1과 2로 시작하는 경우 이 수열은 아래와 같습니다.

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...
짝수이면서 4백만 이하인 모든 항을 더하면 얼마가 됩니까?

## Source code
```java
package com.github.doohong.no2;

public class No_2 {

	public static void main(String[] args) {
		
	   int x,y,z,sum;
	   x=1;
	   y=2;
	   z=3;
	   sum=0;
	   while(x<4000000){
			x=y;
	  		y=z;
	    	z=x+y;
	    	if(x%2==0){
	    		sum=sum+x;
	  		} 
	   }
	System.out.println(sum);		
		// TODO Auto-generated method stub

	}

}
```




