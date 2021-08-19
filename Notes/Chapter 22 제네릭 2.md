# Chapter 22 제네릭 2

## 22-1 제네릭의 심화 문법

와일드카드부터 더 어려워진다... 

### 제네릭 클래스와 상속

제네릭 클래스도 상속이 가능하다. 아래 예제에서 처음으로 제네릭 클래스의 생성자가 등장하는데, 일반적인 생성자와 다른 것은 없다.

<img src="C:\Users\00_ma\Desktop\　　　\3-s\Java basic - github\Ch22.jpg" style="zoom:20%;" />



### Target Types

앞서 컴파일러가 생략된 자료형 정보를 유추한다고 했다. 컴파일러가 자료형 유추를 진행하는 상황이 생각보다 다양하다.

### Wildcard

Object와 String이 상속 관계에 있더라도 `Box<Object>`와 `Box(String)`은 상속 관계를 형성하지 않는 별개의 자료형이다.

```java
public static void peekBox(Box<?> box) {
    System.out.println(box);
}
```

물음표 기호로 표시되는 와일드카드를 이용하여 메소드의 매개변수를 선언할 수 있다. 이렇게 하면 `Box<T>`를 기반으로 생성된 박스인티저, 박스스트링 인스턴스들을 인자로 받을 수 있다. 

```java
public static <T> void peekBox(Box<T> box) {
    System.out.println(box);
}  // 제네릭 메소드의 정의
// 위의것은 와일드카드 기반 메소드 정의
```

기능 측면에서 위 두개의 메소드는 완전히 동일하나, 와일드카드 기반 메소드 정의가 코드가 조금 더 간결해서 선호한다.

### 와일드카드의 상한과 하한의 제한 Bounded Wildcards

#### Upper-Bounded Wildcards

`Box<? extends Number> box`

* box는 `Box<T>` 인스턴스를 참조하는 참조변수이다.
* 이 때 `Box<T>` 인스턴스의 T는 Number 또는 이를 상속하는 하위 클래스여야 함.
  * 예: 박스 넘버, 박스인티저, 박스 더블

#### Lower-Bounded Wildcards

`Box<? super Integer> box`

* box는 `Box<T>` 인스턴스를 참조하는 참조변수이다.
* 이 때 `Box<T>` 인스턴스의 T는 Integer 또는 Integer가 상속하는 클래스여야 함.
  * 예: 박스인티저, 박스 넘버, 박스 오브젝트

### 와일드카드 제한의 목적

필요한 만큼만 기능을 허용하여, 코드의 오류가 컴파일 과정에서 최대한 발견되도록 한다.

#### 상한 제한

```java
public static void outBox(Box<Toy> box) {
    box.get();  // 꺼내는 것 ok
    box.set(new Toy())  // 넣는 것... 이것도 ok!
}
```

outBox에서 넣는 동작을 하는 것은 설계 상 잘못됐지만 이것이 문법적으로는 정상이기 때문에 컴파일 시에 잡히지 않는다.

다음과 같이 매개변수 선언을 하면 꺼내는 것만 가능하고 넣는 것은 컴파일 오류가 나게 된다.

```java
public static void outBox(Box<? extends Toy> box) {
    box.get();  // 꺼내는 것 ok
    box.set(new Toy())  // 넣는 것 ERROR!
}
```

set이 불가능한 이유... box의 타입인자로는 Toy와 그 하위 클래스만 가능하다.

자바의 다형성에 의해, new Toy() 는 Toy보다 상위 클래스 형의 참조변수로 받을 수 있다. Object o = new Toy() 이게 됨. 이런 경우에는 box의 타입인자 조건과 어긋나게 된다. 따라서 new Toy() 하고 새 인스턴스를 생성하는 동작이 막힌다.

또, box의 타입인자로 Toy를 상속하는 하위 클래스인 Car 같은 게 들어올 수 있다. 그럼 `Box<Toy> = box` 이렇게 됨. 이 상황에서 box에 `new Toy()`로 반환되는 인스턴스를 넣을 수 없다.

즉  위와같은 매개변수 선언을 보았을 때, 이 안에서는 <u>box가 참조하는 인스턴스를 대상으로 저장하는 기능의 메소드 호출은 불가능하다</u>라고 알 수 있어야 한다.

#### 하한 제한

```java
public staic void inBox(Box<? super Toy> box, Toy n) {
    box.set(n);  // 넣는 것, ok
   	Toy myToy = box.get();  // 꺼내는 것, Error
}
```

box.get()으로 나온 인스턴스의 반환형을 Toy라고 결정할 수 없어 에러가 생긴다. Toy 보다 더 상위 클래스의 인스턴스가 나올수도 있는 거시다~ (왜냐하면 box의 타입인자는 Toy의 super 즉 Toy와 그 상위클래스니깐..) 그런데 그걸 Toy myToy에 담을 수는 없다. 따라서 에러.

즉 위와같은 매개변수 선언을 보았을 때, 이 안에서 <u>box가 참조하는 인스턴스를 대상으로 꺼내는 기능의 메소드 호출은 불가능하다</u>라고 알 수 있어야 한다.



> 참고: 참조변수를 Object 형으로 선언한다면?
>
> 위의 코드에서 Object myToy = box.get(); 이렇게 선언하면 컴파일 오류 없이 잘 된다. 
>
> 그러나 자바는 Object형 참조변수나 형변환이 불필요하도록 문법을 개선시켜왔다. 즉, 코드에서 Object가 등장할 일을 줄여 왔다. 이는 컴파일러에 의해 오류를 최대한 많이 발견하기 위함이다.
>
> 즉 설명하는 내용에서 위 상황은 논외이고, 당연히 피해야 하는 일이기도 하다.

### 제네릭 인터페이스의 정의와 구현

인터페이스 역시 클래스와 마찬가지로 제네릭으로 정의할 수 있다.

제네릭 인터페이스를 구현할 때는 T를 결정한 상태로 구현할 수도 있다.

단 이렇게 하면 구현 시에 결정된 클래스 이름을 명시하여 구현해야 한다.

책 539p

