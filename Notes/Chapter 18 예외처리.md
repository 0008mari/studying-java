# Chapter 18 예외처리

오류 vs. 예외

* 오류는 문법적 실수. 대부분의 오류는 컴파일 과정에서 그 잘못이 드러남.
* 예외는 프로그램 실행 중 발생하는 정상적이지 않은 상황

## 18-1 자바 예외처리의 기본

### 자바에서 말하는 예외

예외 = 프로그램 실행 중에 발생하는 예외적인 상황. 단순한 문법 오류 x, 실행 중간에 발생하는 정상적이지 않은 상황. 

예시 코드: 나눗셈 함수를 작성하였음. 그런데 사용자가 나누는 수에 0을 입력. 예외 발생 순간에 프로그램이 종료된 사실에 주목하자.

java.lang.ArimeticException: / by zero

이는 가상머신이 예외상황을 처리하는 방식이다. 즉, 가상머신은 예외가 발생하면 그 내용을 간단히 출력하고 프로그램을 종료해버린다.

그리고 위 예제는 실행 과정에서 다른 예외가 발생할 수도 있다. 사용자가 숫자가 아니라 문자를 입력하는 행위 역시 예외이다.

이 경우에도 예외 발생 순간에 프로그램이 종료되었다. 우리는 이 경우에 최소한 예외의 원인을 설명하고 인사 정도는 하고 종료시키고 싶다! 그렇다면 예외의 처리를 가상머신이 아니라 우리가 하면 된다.

### 예외의 처리를 위한 try~catch

앞서 예외가 발생하는 두 상황에서 출력된 메시지를 보면, 다음 클래스의 이름을 확인할 수 있다.

java.lang.ArimeticException

​	수학 연산에서의 오류 상황을 의미하는 예외 클래스

java.util.InputMismatchException

​	클래스 Scanner를 통한 값의 입력에서의 오류 상황을 의미하는 예외 클래스 

이렇듯 자바는 예외 상황별로 그 상황을 알리기 위한 클래스를 정의하고 있다. 이러한 클래스를 가리켜 예외 클래스라 한다. 오류가 발생하면 거기에 해당하는 예외 클래스의 인스턴스가 생성된다. 이 인스턴스를 프로그래머가 처리하면 예외는 처리된 것으로 간주하여 프로그램을 종료하지 않는다. 그러나 처리하지 않으면 프로그램은 그냥 종료가 된다.

예외 처리의 기본 try~catch

```java
try {
    ...관찰영역...
}
catch(Exception name) {
    ...처리영역...
}
```

try와 catch는 하나의 문장이므로 꼭 같이 있어야 한다. 

try 영역에서 발생한 예외 상황을 catch 영역에서 처리한다.

자세히 보면 catch 영역은 메소드처럼 생겼고, 실제로 메소드처럼 동작한다.

예를 들어 Exception name 부분에 ArimeticException e 이렇게 쓸수가 있는데, 아리메틱 익셉션의 인스턴스가 생성된 그것을 catch에서 인자로 받는거다. catch 몸체에서는 뭔짓을 하든 가상머신은 예외가 처리된 것으로 간주한다.

### try로 감싸야 할 영역의 결정

```java
try {
    1. ...
    2. 예외 발생 지점
    3. ...
}
catch(Exception e) {
    ...
}
4. 예외 처리 이후 실행 지점
```

2에서 예외 발생하면 3 무시하고 예외처리 후 4로 넘어간다. 이런 실행 순서를 고려하여 try catch 문을 적절히 이용하여야 한다.

예를 들어서 정수 n1과 n2를 각각 nextInt() 로 받는다고 해보자. (Scanner 이용) n1에서 입력 오류가 나면  n2를 받는 의미가 없다. 따라서 n1과 n2를 구성하고 받는 + 출력하는 과정까지 하나의 작업으로 볼 수 있다. 이를 진행하는 과정에서 어느 한 곳에 예외가 발생하면 나머지 부분을 건너뛰는 것이 적절하다.

### 둘 이상의 예외를 처리하기 위한 구성

try 구문 밑에 catch 구문 두개 쓰는거 가능. 오류의 종류 (즉 인스턴스의 자료형 - 오류클래스명) 에 따라 인자가 구분된 함수처럼...

그런데 | (or)를 이용하여 하나의 catch 문 안에서 여러종류의 예외를 처리하는 것도 가능함. 오류의 종류에 상관 없이 처리하고 싶을 때 묶을 수 있음.

### Throwable 클래스와 예외처리의 책임 전가

자바의 최상위 클래스인 java.lang.Object를 제외하고, 예외 클래스의 최상위 클래스는 다음과 같다.

``java.lang.Throwable`` 

이 클래스에는 발생된 예외의 정보를 알 수 있는 메소드가 정의되어 있다. 대표적인 메소드 둘:

``public String getMassage`` 예외의 원인을 담고 있는 문자열을 반환

``public void printStackTrace`` 예외가 발생한 위치와 호출된 메소드의 정보를 출력

예시

발생된 위치에서 예외 처리가 안 될 경우 그걸 호출한 함수로.. 이렇게 넘어감 끝은 main

### 예외상황을 알리기 위해 정의된 클래스의 종류

대표적인 예외 클래스와 해당 예외가 발생하는 상황.

ArrayIndexOutOfBoundsException 배열 접근 시 잘못된 인덱스 값 사용

ClassCastException 허용할 수 없는 형 변환을 강제로 진행

NullPointerException null이 저장된 참조 변수를 대상으로 메소드 호출 

## 18-2 예외처리에 대한 나머지 설명들

자바에서는 예외 처리 관련 내용이 많다... 차지하는 부분도 크다.

### 예외 클래스의 구분

예외 클래스의 최상위 클래스: Throwable

이를 상속하는 예외 클래스는 다음과 같이 세 부류로 나뉜다.

* Error 클래스를 상속하는 예외 클래스
* Exception 클래스를 상속하는 예외 클래스
* RuntimeException 클래스를 상속하는 예외 클래스
  * RuntimeException 클래스는 Exception 클래스를 상속한다.

#### Error 클래스를 상속하는 예외 클래스

이 중에서 첫번째 케이스(Error 클래스를 ~)의 예와 발생 상황

* VirtualMachineError 가상머신에 심각한 오류 발생
* IOError 입출력 관련해 코드 수준 복구가 불가능한 오류 발생

이 두가지 모두 처리할 수 있는 예외가 아니다. 따라서 이런 유형의 예외가 발생하면 그냥 프로그램 종료시키고 이후 원인 파악한다.

#### RuntimeException 클래스를 상속하는 예외 클래스

앞서 보여준 예시들

* ArimeticException
* ClassCastException
* IndexOutOfBoundsException
* NegativArraySizeException 배열 생성 시 길이를 음수로 지정하는 예외의 발생
* NullPointerException
* ArrayStoreException 배열에 적절치 않은 인스턴스를 저장

이들 예외 역시 대부분의 경우 프로그래머가 예외처리를 하지 않는다.

배열의 길이를 음수로 지정하는 건 프로그래머인데 코드를 수정해야 할 상황이지 예외처리를 할 상황이 아니다.

#### Exception 클래스를 상속하는 예외 클래스

RuntimeException 제외하고 생각해보자

RuntimeException을 직, 간접 상속하지 않고 Exception 클래스만 상속하는 예외클래스!

수로만 따지면 Exception을 상속하는 예외 클래스가 가장 많다. 그리고 이들은 특정 클래스 또는 메소드에 연관되어 있어서, 이들을 나열하여 설명하는 것은 의미가 없다. 앞으로 공부하면서 이 부류에 속한 예외 클래스들을 하나씩 접하게 된다. 그런데 이 부류는 앞서 설명한 두 부류에 속한 예외와는 차이를 보이는 부분이 있는데, 이에 대한 내용을 이어서 설명하자. (아래로)

### Exception을 상속하는 예외 클래스의 예외처리

Exception을 상속하는 예외 클래스 중 비교적 빨리 접하게 될 클래스는 다음과 같다.

java.io.IOException

예시를 보자.

이 종류의 예외처리는 선택이 아니라 필수! try~catch 문으로 처리하거나 다른 영역으로 넘긴다고 반드시 명시해야 한다.

```java
public static void md2() throws IOException {
    // IOException 예외 발생하면 넘긴다!
}
```

코드 작동을 보면 md2에서 md1로 예외를 전달하고, md1에서 예외를 처리한다.

아니면 md1에서도 같은 방식으로 main에게 넘길 수 있다.

main 메소드도 예외를 넘기면, 이 예외는 main을 호출한 가상머신에게 넘어간다. 그러면 플그램은 종류가 된다.

throws 선언을 통해서 둘 이상의 예외를 넘긴다는 표시를 할 수 있다. 컴마로 구분한다.

이런식으로 Exception 클래스를 상속하는 예외는 꼭 처리를 해주어야 하며, 자바에서 예외처리가 필수인 이유이다.

### 프로그래머가 정의하는 예외

지금까지 소개한 예외 클래스는 모두 자바에서 정의한 클래스였다. 그러나 프로그래머가 직접 예외 클래스를 정의하고, 이를 기반으로 특정 상황에서 예외가 발생하도록 할 수도 있다.

예시

```java
import java.util.Scanner;

class ReadAgeException extends Exception {
    public ReadAgeException() {
        super("유효하지 않은 나이가 입력되었습니다.");
    }
}

class MyExceptionClass {
    //main
    ...
    //예외 발생
    public static int readAge() throws ReadAgeException {
        Scanner kb = new Scanner(System.in);
        int age = kb.nextInt();
        
        if (age < 0)
            throw new ReadAgeExcepion(); // 예외의 발생
        
        return age;
    }
}
```

위에서 정의한 예외 클래스 - ReadAgeException

생성자에서 super(상위 클래스의 생성자)를 호출하면서 문자열을 전달함. 이 문자열은 Throwable클래스에 정의된 다음 메소드 호출 시 반환이 된다.

``public String getMessage()``



if (age < 0) 부분은:

정수를 입력받았는데 그 수가 음수인 건 문법적으로 오류가 아니다. 그러나 프로그램 내용 상으로는 그 수가 사람의 나이이므로 오류가 맞다. 이러한 상황을 예외로 처리하기 위해서 예외 클래스를 직접 정의하였다. new로 인스턴스 생성하고 그 인스턴스를 throw 하는 거임~

### 잘못된 catch 구문의 구성

예외 클래스 간 상속관계인 경우. 부모부터 배치하면 부모에서 그 자식에 해당하는 예외까지 잡아낼 수 있으므로, 아래에 작성한 자식 오류처리에까지 오류가 넘어가지 않는다. (프로그래머의 의도와 다르게 동작)

따라서 상속관계의 예외 클래스는 순서를 자식부터 해야한다.

### finally 구문

```java
try {...
}
finally {...
}
// 코드의 실행이 try 안으로 진입하면, 무조건 finally 부분이 실행된다.

try {
    ...
}
catch(Exception name) {
    ...
}
finally {
    ... // 코드의 실행이 try 안으로 진입하면, 무조건 실행된다.
}
```

이렇게 등장하는 finally 구문은 try면 무조건 실행 (예외 발생, catch 실행 여부와 상관 엇음)

finally는 어떻게 유용하게 사용할 수 있을까?

예를 들어서 작업의 마지막에 해야 하지만, 생략되어서는 안 되는 그것!

파일을 닫는 작업이 그 예시이다.

### try-with-resources 구문

위의 try-catch-finally 구문은 코드가 복잡해질 수 있다. 왜냐면 IOException 예외가 close 에서 발생할 수 있는데, 그러므로 finally 부분에 try-catch를 한번 더 넣어줘야 한다. (close 에 대한 예외처리 -필수)

자바7에서 try-with -resource 등장해서 더 간결해졌다

```java
try( resource ) {
    ...
}
catch(Exception name) {
    ...
}
```

위의 try 구문에 메소드같은 부분이 생겼다. 그러나 메소드도 아니고 비슷하지도 않다.

resource 위치에서, **종료의 과정을 필요로 하는** 리소스를 생성할 수 있다. 그러면 이 리소스는 try-with-resource 문을 빠져나오면서 자동으로 종료가 된다.

```java
try(BufferedWriter writer = Files.newBufferedWriter(file)) {
    writer.write('A');
    writer.write('Z');
}
catch(IOException e) {
    e.printStackTrace();;
}
```

이로서 writer가 참조하는 인스턴의 종료는 신경 안 써도 된다. writer.close(); 이걸 직접 안 해줘도 되고 자동으로 됨.

리소스의 종료 관련 메소드는 다음 인터페이스에 나와 있다.

``java.lang.AutoCloseable``

try-with-resource문에 의해 자동 종료되어야 할 리소스 관련 클래스를 이 인터페이스에 구현하여야 한다. 

또, resource 부분에 세미콜론으로 리소스를 구분하여 여러개 리소스를 구성할 수도 있다.

끝으로, 앞으로 나올 예외처리 예제에서는 사용가능한 경우 try-with-resource 문을 적극 활용한다.

### 참고

try 구문 안의 코드는 실행 속도가 느리다. 따라서 불필요한 코드를 try 안에 넣지않는다.

규모가 클수록, 성능이 중요시될수록 try~catch 문 이외에 다양한 방법으로 예외를 처리한다.