# Chapter 19 자바의 메모리 모델과 Object 클래스

## 19-1 자바 가상머신의 메모리 모델

### 가상머신은 운영체제 위에서 동작한다

자바 가상머신의 실행에 필요한 메모리는 운영체제에서 할당한다. 자바 가상머신은 운영체제가 할당해준 메모리 공간을 기반으로, 스스로를 실행하고 + 자바 응용프로그램의 실행을 돕는다.

### 자바 가상머신의 메모리 구성

효율적인 메모리 사용을 위해 영역을 나눈다.

메소드 영역, 스택 영역, 힙 영역 

* 메소드 영역: 메소드의 바이트코드, static 변수
* 스택 영역: 지역변수, 매개변수
* 힙 영역: 인스턴스

### 메소드 영역

Bytecode: 바이트코드. 소스파일을 컴파일 할때 생성되는 코드. 자바 가상 머신에 의해 실행이 가능한 코드.

메소드 영역은 특정 클래스의 정보가 메모리 공간에 올려질 때 채워지는 영역이다.

### 스택 영역

지역변수, 매개변수 저장. 

이 둘의 공통점: local 하다. 중괄호로 구분되는 지역 내에서만 유효.

영역 벗어나면 소멸~

### 힙 영역

인스턴스의 소멸 시점과 방법이 지역변수와 다르기 때문에 힙 영역에 저장한다.

힙에 생성된 인스턴스는 가상머신이 알아서 적절한 시점을 판단하여 소멸시켜준다.

즉, c++ 같은 언어처럼 프로그래머가 명시적으로 메모리 해제를 해줄 필요가 없다. 만약에 이것을 원하는 경우 일반적으로 null을 선언해준다.

추가로 훑어본 문서:

https://mangkyu.tistory.com/118

가비지 컬렉션의 개념 및 동작 원리

### 자바 가상머신의 인스턴스 소멸시키기

예를 들어서... 댕글링 레퍼런스

## 19-2 Object 클래스

모든 자바 클래스의 최상위 클래스인 Object 클래스!

인스턴스를 다루기 위한 메소드가 Object 클래스에 정의되어 있다 

### 인스턴스 소멸 시: finalize 메소드

``protected void finalize() throws Throwable``

아무도 참조하지 않는 인스턴스가 가비지 컬렉션에 의해 소멸되기 전에 자동으로 호출되는 메소드이다.

따라서 인스턴스 소멸 시 반드시 실행해야 할 코드가 있다면 이 메소드의 오버라이딩을 고려할 수 있다.

C++ 이랑 비교해서 자바는 소멸자가 없죠.. 대신에 이 finalize 메소드 오버라이딩으로 소멸자와 같은 효과.

```java
@Override
protected void finalize() throws Throwable {
    super.finalize(); // 상위 클래스의 finalize 메소드 호출
    System.out.println("destroyed: "+ name);
}
```

(name은 해당 메소드가 정의된 클래스 내부의 인스턴스 변수)

상위 클래스의 메소드를 오버라이딩 하는 경우, 오버라이딩 된 메소드를 호출하는 것이 안전하다.

그런데 실행 결과를 보면 finalize 메소드가 호출이 되지 않았음을 알 수 있다. 그 이유는 다음과 같다. 

* 가비지 컬렉션은 빈번히 일어나지 않는다.
* 소멸할 인스턴스가 생겨도 가비지 컬렉션으로 바로 이어지지 않는다.

게다가 실행 중인 프로그램이 종료되면 프로그램을 위해 할당한 메모리가 통째로 사라져서 finalize 메소드가 호출이 생략이 될 수도 있다. 이런 상황에서 System 클래스의 메소드를 통해 어느정도 가비지 컬렉션을 유도할 수 있다.

* ``public static void gc``(): 가비지 컬렉션의 수행을 요청
* ``public static void runFinalization()``: 소멸이 보류된 인스턴스의 finalize 메소드 호출을 요정 (명령이 아닌 요청)

이 메소드들은 요청일 뿐 명령이 아니므로 바로 가비지 컬렉션이 진행되는 것이 아니다. 

자바 가상머신은 매우 합리적인 방법으로 가비지 컬렉션을 수행한다. 따라서 특별한 상황이 아니면 호출할 일 xxxx

### 인스턴스의 비교: equals 메소드

``== 연산자``는 참조변수의 참조 값을 비교한다. 따라서 서로 다른 두 인스턴스의 내용을 비교하려면 별도의 방법을 사용해야한다. 

특정 클래스에 대해 Object클래스의 equals 메소드를 오버라이딩하는 함수를 정의한다. 그 함수에 직접 어떻게 비교할지 작성해야 한다. true, false를 어떻게 반환할지!

자바에서 제공하는 표준 클래스의 경우 equals 메소드가 내용 비교를 하도록 이미 오버라이딩 되어 있는 경우가 많다. 예: String 클래스

### 인스턴스 복사(복제): clone 메소드

``protected Object clone() throws CloneNotSupportedExcception``

이 메소드가 호출되면 호출된 메소드가 속한 인스턴스의 복사본이 생성되고, 그 참조값이 반환된다.

단, interface Cloneable을 구현한 클래스의 인스턴스를 대상으로만 clone 가능.

Cloneable 인터페이스가 바로 마커 인터페이스이다.

인스턴스 복사의 허용 여부는 클래스를 정의하는 과정에서 고민하고 결정해야 한다. 

```java
class Point implements Cloneable {
    private int xPos;
    private int yPos;
    
    public Point(int x, int y) {
        xPos = x;
        yPos = y;    }
}
... 생략
    
```

복사 형태는 p.431 그림과 같다. 

### 인스턴스 변수가 String인 경우의 깊은 복사

String 은 클로너블 하지 않아서 깊은복사 불가

혹시 복사할 클래스에 끼어 있다면 서로 다른 인스턴스에서 String 클래스를 하나 공유하도록 한다 .

### clone 메소드의 반환형 수장: Covariant Return Type

clone() 메소드의 반환형은 Object이다. 따라서 clone()메소드를 호출하면서 형변환도 동시에 진행해야 한다.

```java
Point org = new point(1, 2);
Point cpy = (Point)org.clone();  // 형 변환해야 함
```

자바5부터는 오버라이딩 과정에서 반환형의 수정을 허용한다. 예를 들어서

```java
class AAA {
    public AAA method() {...}  // 반환형이 자신이 속한 AAA 클래스 형
}
// 다음과 같이 반환형 수정 가능
class ZZZ extends AAA {
    @Override
    public ZZZ method() {...}  // 반환형이 자신이 속한 ZZZ 클래스 형
}
```

무엇이든 수정 가능한 건 아니고 위에 처럼 클래스의 이름 으로만 수정 가능 

이러한 문법적 특성을 고려하여 Point 클래스의 clone 메소드는 다음과 같이 오버라이딩 할 수 있다.

```java
class Point implements Cloneable {
    ...
    @Override
        public Point clone() throws CloneNotSupportedException {
        return (Point)(super.clone());
    }
}
```

이렇게 오버라이딩을 하면, 다음과 같이 형 변환 없는 clone 메소드의 호출이 가능하다.

```java
Point org = new Point(1, 2);
Point cpy = org.clone();  // 형 변환 필요 없음
```



