# Chapter 21 제네릭 1

## 21-1 제네릭의 이해

### 제네릭 이전의 코드

제네릭이 갖는 의미는 '일반화'이다. 일반화의 대상은 자료형이다. 제네릭 이전과 이후의 코드 비교부터 살펴보자!

이전에: Object 형을 이용하면 Object를 상속하는 인스턴스를 담을 수 있다. 그러나 꺼낼 때 형변환이 필수고, 실수가 생길 수 있다.

* 필요시 형 변환을 해야 한다.
* 자료형과 관련된 프로그래머의 실수가 컴파일 과정에서 드러나지 않는다.

이런 문제점이 있음.

### 제네릭 기반의 클래스 정의하기

제네릭의 등장으로 자료형에 의존적이지 않은 클래스를 정의할 수 있게 되었다.

```java
class Box {
    private T ob;
    public void set(T o) {
        ob = o;
    }
    public T get() {
        return ob;
    }
}
```

자료형의 종류 T는 인스턴스 생성 시 결정하면 된다. 이와 같이, 인스턴스 생성 시 T의 자료형을 결정하는 것이 제네릭이다. 이것을 컴파일러에 전달하기 위해 문법적으로 아래와 같이 표기한다.

```java
class Box<T> {
    private T ob;
    public void set(T o) {
        ob = o;
    }
    public T get() {
        return ob;
    }
}
```

이 클래스를 대상으로 인스턴스를 생성하는 다음 문장들을 보자.

``Box<Apple> abox = new Box<Apple>();``

* T를 Apple로 결정하여 인스턴스 생성.
* Apple 또는 Apple을 상속하는 하위 클래스의 인스턴스 저장 가능.

#### 용어 정리

* 타입 매개변수: ``Box<T>`` 클래스에서 사용된 ``T``. 메소드의 매개변수와 유사하게 자료형 정보를 인자로 전달받는 형태.
* 타입 인자: ``Box<Apple> abox = new Box<Apple>();`` 해당 문장에서의 Apple. Apple을 타입 매개변수 T에 전달되는 인자로 바라보고 지은 이름
* 매개변수화 타입(제네릭 타입): ``Box<Apple>`` 이것을 매개변수화 타입이라 한다. 자료형 Apple이 타입 매개변수 T에 전달되어 새로운 자료형이 완성된 것이다.

``Box<타입매개변수T>``

``T에 들어가는 Apple이 타입인자.``

`결합된 Box<Apple> 자체를 매개변수화 타입`

### 제네릭 이후의 코드

형변환 불필요해짐 그냥 get()

자료형 실수가 컴파일 과정에서 드러남 

## 21-2 제네릭의 기본 문법

### 다중 매개변수 기반 제네릭 클래스의 정의

둘 이상의 타입 매개변수에 대한 제네릭 클래스도 정의할 수 있다.

```java
class DBox<L, R> {
    private L left;
    private R right;
    
    public void set(L o, R r) {
        left = o;
        right = r;
    }
    
    @Override
    public String toString() {
        return left + " & " + right;
    }
}
```

일반적으로 타입 매개변수의 이름은 다음과 같은 규칙을 지킨다. 

* 한 문자, 대문자로 이름을 짓는다.
* E: Element
* K: Key
* N: Number
* T: Type
* V: Value

### 기본 자료형에 대한 제한 그리고 래퍼 클래스

제네릭 클래스에 대해 기본 자료형의 이름은 타입 인자로 쓸 수 없다.

`Box<int> box = new Box<int>();` 이것은 컴파일 오류

그러나 기본 자료형에 대한 래퍼 클래스를 이용하여 이렇게 작성 가능하다.

```java
class PrimitivesAndGeneric {
    public static void main(String[] args) {
        Box<Integer> iBox = new Box<Integer>();
        iBox.set(125);  // 오토 박싱 진행
        int num = iBox.get();  // 오토 언박싱 진행
    }
}
```

### 타입 인자의 생략: <>

컴파일러는 프로그래머가 작성하는 제네릭 관련 문장에서 자료형의 이름을 추론가능하다.

`Box<Apple> aBox = new Box<>;`

이렇게 선언하면 이 다이아몬드 모양`<>` 빈 공간에 Apple 이 자동으로 들어간다.

### 매개변수화 타입을 타입 인자로 전달하기

박스를 다시 박스에 넣고 그걸 다시 박스에 넣을 수 있다. ...

`Box<Box<String>> wBox = new Box<>();`

이렇게 보니 `<>` 기호로 타입 인자를 생략 가능한 것이 매우 이득이다.

### 제네릭 클래스의 타입 인자 제한하기

Box 안에 들어가는 것을 제한할 수 있다. `extends` 키워드 이용.

`class Box<T extends Number> {...}`: 인스턴스 생성 시 타입 인자로 Number 또는 이를 상속하는 클래스만 올 수 있음.

이렇게 제한하면 특정한 메소드도 정의하여 사용할 수 있다.

```java
class Box<T extends Number> {
    private T ob;
    ....
    public int toIntValue() {
        return ob.intValue(); // OK!
    }
}c
```

### 제네릭 클래스의 타입 인자를 인터페이스로 제한하기

위에서 extends 클래스 써서 제한한 것처럼 인터페이스로도 제한할 수 있다.

똑같이 extends 인터페이스 이렇게 함

인터페이스에 선언되어 있는 메소드 호출 가능.

`class Box<T extends Number & Eatable>`

이렇게 해서 클래스와 인터페이스 섞어서 다중 제한 가능

### 제네릭 메소드의 정의

클래스 전부가 아닌 일부 메소드에 대해서만 제네릭으로 정의하는 것도 가능하다. 이렇게 정의된 메소드를 제네릭 메소드라 한다.

제네릭 메소드는 인스턴스 메소드 뿐만 아니라 클래스 메소드에 대해서도 정의가 가능하다. (static이든 아니든)

`public static <T> Box<T> makeBox(T o) {...}`

`Box<T>` 앞의 `<T>`는 T가 타입 매개변수임을 알리는 표시이다. 이게 없으면 컴파일러에서 T가 무엇인지 알수 없어 컴파일 오류를 일으킨다.

다음과 같이 호출한다.

`Box<String> sBox = BoxFactory.<String>makeBox("Sweet");`

그런데 이렇게도 할 수 있다.

`Box<String> sBox = BoxFactory.makeBox("Sweet");`

T에 대한 타입 인자 정보가 생략되면 컴파일러에서 인자 String을 보고 T를 유추한다. 이는 기본 자료형의 경우 오토 박싱까지 감안하여 이루어진다.

### 제네릭 메소드의 제한된 타입 매개변수 선언

제네릭 메소드 역시 타입 인자를 제한할 수 있다. 뒤따르는 특성 역시 제네릭 클래스에서 타입 인자가 제한되는 상황과 유사하다.

