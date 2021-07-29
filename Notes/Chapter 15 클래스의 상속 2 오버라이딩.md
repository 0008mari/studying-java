# Chapter 15 클래스의 상속 2: 오버라이딩

## 15-1 상속을 위한 두 클래스의 관계

두 클래스를 언제 상속으로 맺어야 할까?

### 상속의 기본 조건인 'IS-A 관계'

* 하위 클래스는 상위 클래스의 모든 특성을 지닌다.
* 거기에 더하여 하위 클래스는 자신만의 추가적인 특성을 더하게 된다.

예시: 모바일폰과 스마트폰

모바일폰을 스마트폰이 상속한다.

=스마트폰도 (일종의) 모바일폰이다.

* IS-A 관계는 '~은 ~이다.'로 표현되는 관계이다.
* 따라서 상속 관계를 형성하기 위한 두 클래스는 IS-A 관계에 있어야 한다.

## 15-2 메소드 오버라이딩

메소드 오버라이딩: 상위 클래스에 정의된 메소드를 하위 클래스에서 다시 정의하는 것.

### 상위 클래스의 참조변수가 참조할 수 있는 대상의 범위

```java
class SmartPhone extends MobilePhone {...}

SmartPhone phone = new SmartPhone("010-555-777", "Nougat");
// 이렇게 구성할 수 있다.
MobilePhone phone = new SmartPhone("010-555-777", "Nougat");
// 그런데 이렇게 참조할 수도 있다.
```

이렇듯 상위 클래스의 참조변수는 하위 클래스의 인스턴스를 참조할 수 있다.

* 모바일폰을 상속하는 스마트폰도 일종의 모바일폰이다.
  * MobilePhone을 상속하는 SmartPhone 인스턴스는 MobilePhone 인스턴스이기도 하다.
* 따라서 MobilePhone형 참조변수는 SmartPhone 인스턴스를 참조할 수 있다.

추가로

``class SmartPhone extends MobilePhone {...}``

이 관계에서

``new SmartPhone("010-555-7777", "Nougat");``

이 인스턴스는 SmartPhone 인스턴스이면서 동시에 MobilePhone 인스턴스이다.

따라서 

```java
SmartPhone phone = new SmartPhone("010-555-777", "Nougat");
// 이렇게 구성할 수 있다.
MobilePhone phone = new SmartPhone("010-555-777", "Nougat");
// 그런데 이렇게 참조할 수도 있다.
```

두가지 방법으로 참조가 가능하다.

예제

```java
MobilePhone ph2 = new SmartPhone("010-999-333", "Nougat");
// 이렇게 생성하면
```

ph2.answer();

MobilePhone 클래스에 정의된 메소드를 호출하는 것이 가능하다.

ph2.playapp();

그러나 SmartPhone 클래스에 정의된 메소드의 호출은 불가능하다. 참조변수 ph2가 실제 참조하는 인스턴스가 SmartPhone 인스턴스이지만, 불가능!

참조변수 ph2는 모바일폰형 참조변수이다. 이 경우 ph2를 통해 접근가능한 멤버는 모바일폰 클래스에 정의되었거나, 이 클래스가 상속하는(즉 이 클래스보다 상위의) 클래스의 멤버로 제한된다. ph2가 참조하는 인스턴스의 자료형은 상관이 없다!

참조하는 인스턴스의 자료형이 기준이 아니고, 참조변수의 형의 기준이다. 이렇게 설계한 이유는 컴파일 타임에서 형체크를 하기 위해서이다!! 

또, 참조변수의 형을 기준으로 접근가능한 멤버를 제한하는 것은 코드를 단순하게 한다.

### 클래스의 상속과 참조변수의 참조 가능성에 대한 정리

```java
class Cake {
    public void sweet() {...}
}
class CheeseCake extends Cake {
    public void milky() {...}
}

```

이런 관계를 예시로 들어보자.

StrawberryCheeseCake 인스턴스는, 치즈케이크 인스턴스이면서 케이크 인스턴스이다.

따라서 다음과 같은 참조가 가능하다.

```java
Cake cake1 = new StrawberryCheeseCake();
CheeseCake cake2 = new StrawberryCheeseCake();
```

그러나 Cake형 참조변수 cake1로 호출할 수 있는 메소드는 Cake.sweet() 이거뿐.

cake2의 경우 Cake.sweet() 와 CheeseCake.milky() 이렇게 2가지 호출 가능.

이렇듯 호출 가능한 메소드의 기준은 **참조변수의  type**이다. (실제 인스턴스의 형 기준 x)

### 참조변수 간 대입과 형 변환

```java
class Cake {
    public void sweet() {...}
}
class CheeseCake extends Cake {
    public void milky() {...}
}
```

위 상황에서 다음과 같은 형태의  참조변수 사이의 대입은 가능하다. 

```java
CheeseCake ca1 = new CheeseCake();
Cake ca2 = ca1; // 가능

Cake ca3 = new CheeseCake();
// CheeseCake ca4 = ca3; // 불가능

CheeseCake ca4 = (CheeseCake)ca3; //가능
```

이 경우에도, 대입 연산이 가능한지 불가능한지의 기준이 참조변수의 type이다.

마지막줄과 같이 형변환을 하면 가능. 이는 ca3가 참조하는 인스턴스가 치즈케이크라는 걸 프로그래머가 보장하는 경우이다. 따라서 형변환 시 실수가 일어나지 않도록 주의한다.

### 클래스의 상속과 참조변수의 참조 가능성: 배열 관점에서의 정리

위의 케이크->치즈케이크 관계를 또 들고옴

```java
Cake cake = new CheeseCake(); // 이렇게 참조 가능

CheeseCake[] cakes = new CheeseCake[10]; // 가능
Cake[] cakes = new CheeseCake[10];// 가능
```

### 메소드 오버라이딩

메소드 오버라이딩: 상위 클래스에 정의된 메소드를 하위 클래스에서 다시 정의하는 것. 여기서의 오버라이딩은 '무효화 시키다'

오버라이딩 조건

* 메소드의 이름, 메소드의 반환형, 메소드의 매개변수 선언

세가지가 같아야 메소드 오버라이딩 성립.

상속관계 하위 애들이 오버라이딩하면, 가장 하위의 함수로 덮어씌워짐.

호출하는 자료형에 관계 없이 가장 하위의 오버라이딩 한 함수가 호출된다.

### 오버라이딩 된 메소드를 호출하는 방법

가려진 상위의 메소드를 호출하는 방법이다.

클래스 외부가 아닌 내부에서 호출하는 방법이 있다. super 키워드이다.

super. 붙이고 메소드 호출하면 상위 클래스에 정의된 , (오버라이딩된) 메소드가 호출이된다.

cf. super를 그냥 함수처럼 쓰면 super(인자); 상위클래스의 생성자 호출인거 잊지 않았지? 

### 인스턴스 변수와 클래스 변수도 오버라이딩의 대상이 되는가?

안 된 다.

**인스턴스 변수**

참조변수의 형에 따라 접근하는 변수가 결정된다. 

**클래스 변수, 클래스 메소드**

동일하게, 참조변수의 형에 따른다. 

## 15-3 instanceof 연산자

상속과 관련된 연산자

### instanceof 연산자의 기본

```java
if (ca instanceof Cake)
    ....
```

ca는 참조변수, Cake는 클래스 이름이다. 

ca가 참조하는 인스턴스가 Cake의 인스턴스이거나 Cake를 상속하는 클래스의 인스턴스이면 -> true

아니면 -> false

### instanceof 연산자의 활용

참조변수가 참조하는 인스턴스에 따라서, 호출하는 메소드를 달리하는 코드.

```java
class Box {
    public void simpleWrap() {...}
}
class PaperBox extends Box {
    public void paperWrap() {...}
}
class GoldPaperBox extends PaperBox {
    public void goldWrap() {...}
}

// 위 클래스들의 인스턴스를 대상으로 하는 다음 메소드 정의
public static void wrapBox(Box box) {
    ...
}

// 위에서 정의한 세 클래스의 인스턴스 모두, 위 메소드의 인자로 전달될 수 있다. 이렇게 정의하고자 한다.

public static void wrapBox(Box box) {
    // box가 Box 인스턴스를 참조하면, simpleWrap 메소드 호출
    // box가 PaperBox 인스턴스를 참조하면, paperWrap 메소드 호출
    // box가 GoldPaperBox 인스턴스를 참조하면, goldWrap 메소드 호출
}
```

이렇게 하기는 쉽지 않은걸...? instanceof 연산자를 활용한다.

```java
class Box {
    public void simpleWrap() {
        System.out.println("Simple Wrapping");
    }
}
class PaperBox extends Box {
    public void paperWrap() {
        System.out.println("Paper Wrapping");        
    }
}
class GoldPaperBox extends PaperBox {
    public void goldWrap() {
        System.out.println("Gold Wrapping");
    }
}

class Wrapping{
    public static void main(String[] args) {
        Box box1 = new Box();
        PaperBox box2 = new PaperBox();
        GoldPaperBox box3 = new GoldPaperBox();
        
        wrapBox(box1);
        wrapBox(box2);
        wrapBox(box3);
    }
    
    public static void wrapBox(Box box) {
        if (box instanceof GoldPaperBox) {
            ((GoldPaperBox)box).goldWrap();
        }
        else if (box instanceof PaperBox) {
            ((PaperBox)box).paperWrap();
        }
        else {
            box.simpleWrap();
        }
    }
}
```

참조하는 인스턴스 종류(하위-상위 상속관계) 에 따라 호출하는 메소드를 달리하는 코드.

조건을 달 때 가장 하위부터 단다. 왜냐면 instanceof는 해당 인스턴스 혹은 상속하는 인스턴스를 참조하면 true라서.

다음과 같이 이해할 수도 있다: 연산자 instanceof는 명시적 형 변환의 가능성을 판단해주는 연산자이다. 

# 문제 15-1 메소드 오버라이딩

위의 예제를 instanceof 연산자 없이 구성하라. 메소드 오버라이딩 이용

```java
class Box {
    public void wrap() { 
        System.out.println("Simple Wrapping");
    }
}

class PaperBox extends Box {
    public void wrap() {
        System.out.println("Paper Wrapping");
    }
}

class GoldPaperBox extends PaperBox {
    public void wrap() {
        System.out.println("Gold Wrapping");
    }
}

class Wrapping {
    public static void main(String[] args) {
        Box box1 = new Box();
        PaperBox box2 = new PaperBox();
        GoldPaperBox box3 = new GoldPaperBox();
        
        wrapBox(box1);
        wrapBox(box2);
        wrapBox(box3);
    }

    public static void wrapBox(Box box) {
        box.wrap();
    }
}
```

이게뭐지 오버라이딩하면 그것만 나오는거 아닌가

그래서 예제 더 찾아봄...



```java
class Parent {
    void display() {
        System.out.println("부모 클래스의 display()"); 
    }
}

class Child extends Parent {
    void display() { 
        System.out.println("자식 클래스의 display()"); 
    }
}

public class Inheritance05 {
    public static void main(String[] args) {
        Parent pa = new Parent();
        pa.display();
        Child ch = new Child();
        ch.display();
        Parent pc = new Child();
        pc.display(); // Child cp = new Parent();
    }
}
```

결과

부모, 자식, 자식 으로 나온다.

세번째꺼가 쟁점.



아... 애초에 pa는 Parent니까 아래 오버라이딩 부분까지 안가는구나

pc의 경우에는 자바의 **다형성**이라는 특성에 기반함.



그치... Parent의 메소드를 호출하는 것. 하위 클래스 정보조차 알 수가 x...