---
title:  "Scanner와 BufferedReader의 차이"
excerpt: "백준 1000번 문제를 풀다가 공부하게 된 Scanner와 BufferedReader"

categories:
  - Java
tags:
  - Java
  - 프로그래밍
  - 백준
last_modified_at: 2022-03-14T23:34:00
---

# Scanner와 BufferedReader의 차이

이번 글은 백준 1000번 알고리즘 문제를 풀면서 알게된 Scanner와 BufferedReader의 차이점에 대해 알아보겠다.

우선 우리는 일반적으로 자바에서 입력에 대해 처음 배울 때는 Scanner 클래스를 먼저 접하게 된다. Scanner 클래스는 java.utill.Scanner를 import만 하면 간단하게 사용할 수 있기 때문이다.

그러면, 이렇게 간단하게 사용할 수 있는 Scanner를 사용하지 않고 BufferedReader 클래스를 사용하는 이유는 뭘까? 바로 **속도**때문이다.

Scanner와 BufferedReader 모두 버퍼를 이용해서 입력값을 저장하는데 Scanner의 버퍼크기는 1024 chars이고, BufferedReader의 버퍼 크기는 8192 chars로 Scanner보다 8배나 크다.

즉, BufferedReader가 더 많은 입력 값을 한 번에 처리할 수 있기 때문에 속도에서 차이가 난다.

버퍼 크기를 언급한 이유는 BufferedReader가 버퍼를 사용하여 읽기를 하는 클래스이기 때문이다.

버퍼를 사용하지 않는 입력은, 키보드의 입력이 키를 누르는 즉시 바로 프로그램에 전달되는 것이고,

버퍼를 사용하는 입력은 키보드의 입력이 있을 때마다 한 문자씩 버퍼로 전송해뒀다가 버퍼가 가득 차거나 혹은 개행문자가 나타나면 버퍼의 내용을 한 번에 프로그램에 전달한다.

여기서 들었던 생각은 한 번 버퍼를 거쳐 출력되는 것보다, 키보드의 입력을 받는 즉시 출력하는 것이 더 빠른거 아닌가? 하는 생각이 들어서 좀 더 알아보니 아래와 같은 답을 구할 수 있었다.

‘하드디스크는 속도가 느리다. 그리고 외부 장치와 데이터 입출력도 시간이 걸리기 때문에 키보드의 입력이 있을 때마다 바로 이동시키는 것보다 중간에 버퍼를 두어 한 번에 묶어 보내는 것이 더 효율적이고 빠른 방법이다.’

쉽게 생각해서 쓰레기가 있을 때마다 밖에 버리는 것보단 쓰레기통이라는 공간(버퍼)에 하나하나 모았다가 꽉 차면 밖에 버리는 게 훨씬 효율적이라는 개념이다.

Scanner sc = new Scanner([System.in](http://System.in));

위와 같이 Scanner을 이용해서 System에서 입력을 받아올 수 있고,

BufferedReader br = new BufferedReader(new InputStreamReader([System.in](http://System.in)));

여기서 System.in의 뜻은 입력한 값을 Byte단위로 읽으며 키보드와 연결된 자바의 표준 입력 스트림이다.

즉,  InputStreamReader라고 하는 것이 System.in에서 받아온 Byte 문자를 하나 하나 읽어드리고, BufferedReader가 읽어들여온 문자들을 문자열로 만든다.

아래의 코드는 Scanner를 통해 1~100만, 즉 100만개의 데이터를 입력받는 코드이다.

```java
import java.util.Scanner;
public class Main{
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int t;
		sc.nextLine(); // 입력 버퍼를 비워주는 역할을 한다.
		long st1 = System.currentTimeMillis();
		for(int i=0;i<1000000;i++){
			t = sc.nextInt();
		}
		System.out.println("소요시간 "+(System.currentTimeMillis() - st1)+"ms");
		sc.close(); // Scanner 닫기
	}
}
```

숫자 100만개를 입력받는데 걸리는 시간은 대략 2.5초(2441ms)가 소요되었다. 100만개 입력에 2.5초라고 놓고 보면 빠른 것 같지만 데이터가 천만~일억에 이르는 경우나 연산시간까지 고려하면 뛰어난 성능은 아니다.

아래의 코드는 BufferedReader를 이용해 입력받은 코드이다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader; //이 2개 import필수(java.io.*; 로 전부 받을수도 있다.)

public class Main {
    public static void main(String []args) throws Exception { //예외처리 필수
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        String s = br.readLine(); //입력받을값이 String일때
        int a = br.read(); //입력받을값이 int일때
        int b = Integer.parseInt(br.readLine()); //int값+엔터 까지 입력받을때
    }
}
```

위와 같은 형태로 값을 입력받을 수 있는데, 여기서 b는 먼저 스트링으로 개행문자(엔터)까지 포함해 전체를 받아온 다음, 형 변환을 통해 원하는 타입으로 저장했다.

또한, new BufferedReader(new InputStreamReader([System.in](http://System.in)), 1024); 형태로 버퍼 사이즈를 직접 지정할 수도 있다.

BufferedReader를 이용해서 100만개의 값을 입력받는 코드는 아래와 같다.

```java
import java.io.InputStreamReader;
import java.io.BufferedReader;

public class Main{
	public static void main(String[] args) throws Exception {
		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
		in.readLine();
		int t;
		long st2 = System.currentTimeMillis();
		for(int i=0;i<1000000;i++){
			t = Integer.parseInt(in.readLine());
		}
		System.out.println("소요시간 "+(System.currentTimeMillis() - st2)+"ms");
	}
}
```

소요시간은 0.4초(452ms)밖에 걸리지 않았다. 이러한 차이가 발생하는 이유로 buffer size 크기의 차이도 있겠지만, BufferdReader는 데이터를 한 번에 읽어와 버퍼에 보관 후 사용자 요청시 버퍼에서 데이터를 읽어오는 방식으로 시간부하를 줄일 수 있기 때문이다.

따라서 Scanner보다 BufferedReader가 속도면에서 더 큰 장점을 가지고 있으니, 참고하자

그 밖에 다른 차이점으로는 아래와 같다.

1. Scanner은 정규식을 사용하여 값을 파싱한다.

   Scanner은 정수 값으로 int, short, long, 소수 값으로 float, double 등을 nextInt(), nextShort(), nextLong(), nextFloast(), nextDouble()과 정규식 함수를 이용하여 데이터 타입이 결정되므로 별도의 Casting이 필요하지 않지만 반면에 BufferedReader는 오직 String 값만을 읽기 때문에 readLine() 함수만을 사용한다.

2. BufferedReader 경우 동기화를 사용하지만 Scanner은 사용하지 않는다.

   멀티 쓰레드 환경에선 여러 쓰레드가 같은 프로세스 내의 자원을 공유하기 때문에 서로의 작업에 영향을 줄 수 있다. 그렇기 때문에 Scanner는 멀티 쓰레드에 안전하지 않고, BufferedReader는 안전하다.

3. BufferdReader는 IOException 예외 처리를 던지지만, Scanner는 숨긴다.

위의 방식은 enter(개행문자)로 입력받는 형식으로 작성한거고, 연속된 문자 형식으로 입력받는 방법으로는 아래처럼 split이나 StringTokenizer을 사용하면 된다.

```java
import java.io.*; // BufferedReader 사용
import java.util.*; // StringTokenizer 사용

public class BufferEx{
	public static void main(String[] args) throws IOException { // 예외 처리 필수
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
								
		// 1번 방법 : split
		String sp[] = br.readLine().split(" ");

		// 2번 방법 : StringTokenizer
		StringTokenizer st = new StringTokenizer(br.readLine());
		int arr[] = new int[st.countTokens()];
		int count = 0;
		while(st.hasMoreTokens()){
			arr[count] = Integer.parseInt(st.nextToken());
			System.out.println(arr[count++]);
			// countTokens() : 총 토근의 개수를 리턴
			// hasMoreTokens() : 토큰이 남아있다면 true, 없으면 false 리턴								
		}							
	}
}
```

또한 여기서도, split는 정규식을 기반으로 문자열을 자르는 로직으로 동작하기 때문에 단순히 공백 자리를 당겨서 채우는 StringTokenizer가 더 빠르게 동작한다.

https://st-lab.tistory.com/41 해당 사이트를 참고하여 추가하자
