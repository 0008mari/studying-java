# Chapter 10 클래스 변수와 클래스 메소드

## 10-1 static 선언을 붙여서 선언하는 클래스 변수

인스턴스 변수는 인스턴스 생성 시 생기는 변수고, 클래스 변수는 인스턴스의 생성과 관계없이 존재하는 변수이다.

### 선언된 클래스의 모든 인스턴스가 공유하는 '클래스 변수(static 변수)'

static으로 선언된 변수는, 해당 클래스의 모든 인스턴스가 공유하는 변수이다.

어떠한 인스턴스에도 속하지 않음.

메모리 공간에 단 하나만 존재하는 변수.

변수가 선언된 클래스의 인스턴스들이, 이 변수에 접근할 수 있는 권한이 있음.

### 클래스 변수의 접근 방법

* 클래스 내부 접근: 변수의 이름을 통해 직접 접근
* 클래스 외부 접근: 클래스 또는 인스턴스의 이름을 통해 접근

외부에서 접근 가능한가? default니깐 default 에 따라서 가능합니다 (클래스 내부 + 클래스 외부&동일패키지)

### 클래스 변수의 초기화 시점과 초기화 방법

클래스 변수는 인스턴스 생성 이전에 메모리 공간에 존재한다.

따라서 클래스 변수의 옳은 초기화 방법은 다음과 같다.

```java
class InstCnt {
    static int instNum = 100; // 정상적인 초기화 방법.
    InstCnt() {
        instNum = 0; // 이렇게 하면 인스턴스가 생성될때! 매번 0으로 초기화
    }
}
```

### 클래스 변수를 언제 유용하게 활용할 것인가?

인스턴스 간에 데이터 공유가 필요한 상황에서 클래스 변수를 선언한다.

앞선 예시: 생성된 인스턴스의 수를 관리하는 상황.

하나더: 클래스 내부와 외부에서 참조해야 할 정보. 예를 들어서 PI 값을 이렇게 저장할 수 있다. 모든 Circle 인스턴스가 참조해야하는 값이지만, 인스턴스가 각각 지녀야하는 값은 아니기 때문.

참조를 목적으로만 존재하는 값은 final 선언이 된 클래스 변수에 담는다.

## 10-2 static 선언을 붙여서 선언하는 클래스 메소드

메소드에 static 선언을 하면 클래스 메소드가 된다. 클래스 메소드는 성격이 클래스 변수와 유사하다.

### 클래스 메소드의(static 메소드의) 정의와 호출

* 인스턴스 생성 이전부터 접근이 가능하다.
* 어느 인스턴스에도 속하지 않는다.

###  클래스 메소드로 정의하는 것이 더 나은 경우

* 모두 외부에 기능을 제공하기 위한 메소드들이다.
* 모두 인스턴스 변수의 값을 참조하거나 수정하지 않는다.

인스턴스에 따라 달라지는게 아니거,,, 그냥 인풋에 따라 아웃풋이 나오는 단순한 형태.

클래스 메소드로 구성된, 인스턴스 생성을 목적으로 설계되지 않은 클래스들도 존재하고, 프로그래머가 이러한 유형을 설계하는 경우도 종종 있다.

### 클래스 메소드에서 인스턴스 변수에 접근이 가능할까?

네.. 인스턴스 변수에 당연히 안 됩니다.

같은 클래스 변수(static 키워드 붙은) 나 클래스 메소드에는 가능하겠지요.

## 10-3 System.out.println() 그리고 public static void main()

### System.out.println()

* System: 자바에서 제공하는 클래스. Java.lang 패키지 (즉, 풀네임은 Java.lang.System.out.println(...); 그러나 컴파일러가 import Java.lang.*; 자동으로 해준다 ..)

* out은 static으로 선언된 클래스 변수이다. 클래스 이름을 통해 접근하기 때문. 

  ```java
  public final class System extends Object {
      public static final PrintStream out; //참조변수 out
  }
  ```

* println은 PrintStream 클래스의 인스턴스 메소드이다.

종합하자면... System에 위치한 클래스 변수 out... 이거가 참조하는 인스턴스의 println 메소드를 호출하는 문장.

### main은 왜 public, static인가?

일단은 약속이구요. 컴파일러가 public static void main(String[] args)를 찾아서 실행을 합니다.

main의 호출은 클래스 외부에서 이루어지므로, public이겠지요

그리고 main은 인스턴스 생성 이전에 호출되므로, static이다.

### main 메소드를 어디에 위치시킬 것인가?

일반적으로, main 메소드를 담을 목적으로 별도의 클래스를 정의한다. 그러나 main 메소는 static 메소드라서 아무 클래스에나 담아도 된다.

main에서 Car 인스턴스를 생성하는데, Car 클래스 내에 main 메소드를 담는다고? 난해하다면 다음과 같은 사실을

* Car 클래스와 static main 메소드는 사실상 별개. Car 클래스가 공간을 제공했을 뿐이다.

이렇게 하면 실행 시에는 

```
C:\JavaStudy>Java Car 
```

이렇게 실행한다. 해당 예시의 다른 클래스인 Boat 클래스에 두어도 되고, 이 경우엔 Java Boat 이렇게 실행한다.

## 10-4 또 다른 용도의 static 선언

사용 빈도가 높진 않으나 알아두자

### static 초기화 블록

static 변수를 생성하면서 초기화할 수 있도록 블록이 존재한다. 즉, 실행시간 상 static 변수와 동일하게, JVM이 클래스 정보를 읽어들일 때 실행이 된다.

```java
import Java.time.LocalDate;

class DateOfExecution {
    static String date;
    
    static { //클래스 로딩 시 단 한 번 실행이 되는 영역
        LocalDate nDate = LocalDate.now();
        date = nDate.toString();
        // 오늘 날짜 얻어오는 코드~
    }
    
    public static void main(String[] args) {
        System.out.println(date);
    }
}
```

### static import 선언

앞서 클래스변수의 예시로 PI를 들었는데, 이건 Java.lang.Math에 클래스 변수로 선언되어 있다. 

System.out.println(Math.PI);

앞서 언급하였듯 Java.lang.*이 임포트가 자동으로 되어있어서 (컴파일러가 알아서 삽입해줌) 이렇게 쓸수 있는 것.

근데 이 Math도 생략할 수 있는 방법이 있다.

import static Java.lang.Math.PI; // PI에 대한 static import 선언

이렇게 하면 된다... 

자주 사용하면 선언 어디에 했는지 구분이 힘들어서 방해가 될 수 있다.