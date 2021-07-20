# Chapter 3 상수와 형 변환

## 3-1 상수

상수: '값이 변하지 않는 수', '한번 값이 정해지면 이후로 변경이 불가능한 변수'

### 자바의 일반적인 상수

변수 선언 시 final 붙이면 상수가 됨 (값을 딱 한 번만 할당 가능. 할당된 값 변경 불가)

```java
package javastudy;

public class helloWorld {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println("Hello, world!");
	}
	

}

class Constants {
    public static void main(String[] args){
        final int MAX_SIZE = 100;
        final char CONST_CHAR = '상';
        final int CONST_ASSIGNED;
        CONST_ASSIGNED = 12;
        
        System.out.println("상수1: " + MAX_SIZE);
        System.out.println("상수2: " + CONST_CHAR);
        System.out.println("상수3: " + CONST_ASSIGNED);
        
    }
}
```

이클립스에서 Java 파일 실행하는 법 처음 해봤다 

Class 생성 버튼 눌러서 클래스 추가하고, main(String[] args) 선택해야됨...

실행할때는 해당 자바 파일 우클릭 -> Run as application



관례 상 상수의 이름 짓기 규칙이 있다.

* 상수의 이름은 모두 대문자로 짓는다.
* 상수 이름 단어 사이에 언더바를 넣는다.



### 리터럴(Literals)에 대한 이해

``int num = 157; //숫자 157은 리터럴 상수``

컴파일러는 157을 int형 정수로 인식한다 

long 형 변수에 157 이렇게 정수를 넣으면 int 라서 형 안맞는다고 나온다 ;; 그럼 어떻게 하냐? 12478123781L 이렇게 엘을 붙인다.

* long: []L
* binary: 0B[]
* 읽기 편하기 위해 정수 중간에 언더바 허용 _

실수는 기본이 더블. []D 이렇게 써도 됨

3.4e3 이렇게 써도 됨 (= 3.4 * 10^3)

* float: []f

이외에도 

* bool: true, false 2가지만 존재
* 이스케이프 시퀀스
* 유니코드 문자: ``\u20AC`` 20AC에 해당하는 유로화 기호 출력

## 3-2 형변환

### 자동 형변환 (implicit)

연산 시의 형변환.

* 자료형의 크기가 큰 방향으로 형변환이 일어난다.
* 크기와 상관 없이 정수 자료형보다 실수 자료형이 우선한다.

즉, long이 float으로 변환될 수 있다.

실수형 데이터의 표현 범위가 더 넓기 때문에, 정수형->실수형에서의 데이터 손실 x. 오차는 존재

### 명시적 형변환 (explicit)

``int num2 = (int)num1;``

더 작은쪽으로 형변환하면 상위 바이트가 잘려나간다.

위의 예시에서 num1 은 8바이트의 long 형인데, 상위 4바이트가 짤리고 하위 4바이트가 int num2에 저장된다.

