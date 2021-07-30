# Chapter 17 인터페이스와 추상 클래스

## 17-1 인터페이스의 기본과 그 의미

인터페이스가 무엇인지. 세세한 문법 설명에 앞서, 의미부터

### 추상 메소드만 담고 있는 인터페이스

```java
interface Printable {
    public void print(String doc);	// 추상 메소드
}
```

클래스와 기본 골격 동일. interface 키워드 선언. 메소드는 몸체 없이 세미콜론으로 마무리됨

몸체가 비어있는 메소드를 가리켜 **추상 메소드**라고 함. 이 인터페이스를 대상으로는 인스턴스 생성 불가능. 다만, 다른 클래스에 의해 상속될 뿐.

```java
class Printer implements Printable {
    public void print(String doc) {
        System.out.println(doc);
    }
}
```

프린터 클래스에서 Printable 인터페이스를 상속하고, (**implements** 키워드 사용) 메소드를 구현하였다.

클래스가 인터페이스를 상속하는 행위는 '상속' 이 아니라 '**구현**'이라고 한다. 문법 관계는 상속과 동일하나, 본질은 구현이기 때문이다.

클래스의 인터페이스 구현에는 다음과 같은 특징이 있다.

* 구현할 인터페이스를 명시할 때 키워드 implements를 사용한다.
* 한 클래스는 <u>둘 이상의 인터페이스를 동시에 구현</u>할 수 있다.
* 상속과 구현은 동시에 가능하다.

동시에 가능하단 건 아래와 같은 게 가능하단 거

``class Robot extends Machine implements Movable, Runnable {..}``

Machine을 상속하고 또 Movable, Runnable을 구현 (여러개 구현 가능)

* 인터페이스의 형을 대상으로 참조변수의 선언이 가능하다.
* 인터페이스의 추상 메소드와 이를 구현하는 메소드 사이에 오버라이딩 관계가 성립한다. 
  * 따라서 어노테이션 @Override 선언 가능 

``Printable prn = new Printer();``

이때 Printable은 인터페이스. Printer는 Printable을 구현한 클래스. Printer 클래스의 인스턴스의 참조 변수로 Printable 형을 사용하고 있음.

``prn.print("Hello Java");``

Printable 형의 참조변수 prn을 이용하여 메소드를 호출하였다. 메소드 오버라이딩이 적용되어 Printer 클래스의 print 메소드가 호출된다. @Override 어노테이션을 이용해 실수의 확률을 줄일 수 있다. (구현 몸체인 print 함수 정의 윗줄에 붙이는 거 알죠 )

### 인터페이스의 본질적 의미

인터페이스의 사전적 의미: 연결점, 접점.

예시: MS의 윈도우 운영체제에서, 삼성과 LG의 프린터를 대상으로 출력을 진행할 수 있다. 모든 업체의 프린터가 윈도우 운영체제에서 연결될 수 있다! 이것을 주도하는 것은 MS이다. MS는 이런 결정을 내린다.

'인터페이스를 하나 만들어서, 모든 프린터 업체에게 제공해야겠다'

```java
interface Printable {
    public void print(String doc);
}
```

이 인터페이스를 회사별로 각자 구현해서 가져오길 바랍니다.~^^ 그러면 저희가 print 메소드를 호출하면서 출력할 문서의 정보를 인자로 전달하겠습니다.

각 프린트 회사에서 이를 구현하여 오버라이딩 한다!

MS에서는 각 회사의 클래스 이름만 알면 되지, **구현 방법**을 알 필요가 없다. 이것이 인터페이스를 두는 이유이다.

## 17-2 인터페이스의 문법 구성과 추상 클래스

인터페이스에 존재할 수 있는 메소드: 추상 메소드, 디폴트 메소드, static 메소드가 있다. 그리고 인터페이스 간 상속 가능. 인터페이스의 type 이름을 대상으로 instanceof 연산을 할 수도 있다. (클래스와 유사한 특성 다수)

### 인터페이스에 선언되는 메소드와 변수

#### 메소드

**인터페이스의 모든 메소드는 public으로 간주한다.**

즉 접근지정자 안 쓰면 public 이다!

#### 변수

인터페이스 내에도 변수를 선언할 수 있다.

* 반드시 선언과 동시에 값으로 초기화를 해야 한다.
* 모든 변수는 public, static, final이 선언된 것으로 간주한다.

결론적으로, 인터페이스 내에 선언된 변수는 상수이다. final, static 으로 자동 선언된다.

상수는 대문자로 이름을 짓는 관례에 따라, 인터페이스 내에 위치한 변수의 이름은 대문자로 작성한다.

끝으로 

인터페이스를 구현하는 클래스는 인터페이스 내의 모든 추상 메소드를 구현해야 한다. 하나라도 안 하면 해당 클래스를 대상으로 인스턴스 생성이 불가능하다.

### 인터페이스 간 상속

인터페이스를 받는 클래스는 인터페이스 내의 모든 추상메소드를 구현해야 하기 때문에, 인터페이스를 함부로 수정하기 어렵다!! (이미 사용되는 곳에서 전부 수정해야 함!!) 그치만 인터페이스의 내용을 추가해야할 때가 있다!! 

그런 경우에는 상속을 이용!!

* 두 인터페이스 사이의 상속 extends 키워드
* 두 클래스 사이의 상속 extends 키워드

* 인터페이스 - 클래스 사이의 구현 implements 키워드

### 인터페이스의 디폴트 메소드

이런 상황) 이미 활발히 사용되는 수십개의 인터페이스가 있다. 대대적인 기능 보강을 위해 모든 인터페이스에 최소 한개 이상의 추상 메소드를 추가하려고 한다. 이걸 **인터페이스의 상속**으로 처리하면 인터페이스 수가 두배가 되는 문제가 생긴다.. (너무많아) 이런 상황의 해결을 위해 **디폴트 메소드**라는 기능이 자바8에서 도입되었다.

```java
interface Printable {
    void print(String doc);
    default void printCMYK(String doc) { }
    // 인터페이스의 디폴트 메소드
}
```

디폴트 메소드의 특징

* 자체로 완전한 메소드이다.
* 따라서 이를 구현하는 클래스가 <u>오버라이딩 하지 않아도 된다.</u>

그리고 위의 Printable 인터페이스를 보고 다음과 같이 이해할 수 있어야 한다.

* 처음에는 Printable 인터페이스에 print 추상 메소드만 있었구나.
* 이후에 필요에 의해 printCMYK 메소드를 추가하였구나.
* 그래도 디폴트 메소드로 추가하였으나, 이전에 구현된 드라이버에는 영향을 주지 않는구나.

인터페이스에 추상 메소드를 추가하는 상황에서, 이전에 개발해놓은 코드에 영향을 미치지 않기 위해 등장한 문법이다.

즉, 첫 설계부터 디폴트 메소드를 정의해 넣는다면... 잘못 이해하고 사용하고 있는 것이다.

### 인터페이스의 static 메소드 (클래스 메소드)

클래스의 static 메소드와 유사

다른 메소드들과 마찬가지로 public이 선언된 것으로 간주

구현할 필요 없는 메소드

근데 인터페이스 만으로는 인스턴스 생성이 불가해서 예제 보면 그걸 그냥 구현만 하는 클래스가 존재함 (클래스의 몸체는 빈칸 { })

사실 static 쓰는 경우가 별로 없음... 그런데 자바 기본 제공 인터페이스에서 쓰는 경우가 있어서 설명해 둡니다.

* 인터페이스에도 static 메소드를 정의할 수 있다.
* 인터페이스의 static 메소드 호출 방법은 클래스의 static 메소드 호출 방법과 같다.

### 인터페이스 대상의 instanceof 연산

앞서 공부햇던 instanceof 기억하죠?

이와 유사하게 인터페이스 대상으로도 가능.

'해당 인터페이스를 직접, 혹은 간접적으로 구현한 클래스의 인터페이스인 경우' true 반환.

### 인터페이스의 또 다른 사용 용도: Marker Interface

인터페이스는 클래스에 특별한 표식을 다는 용도로도 사용이 가능하다.

이렇게 사용되는 인터페이스를 가리켜 마커 인터페이스라 한다. 마커 인터페이스에는 아무런 메소드도 존재하지 않는 경우가 많다.

```java
interface Upper { } // 마커 인터페이스
interface Lower { } // 마커 인터페이스

interface Printable {
    String getContents();
}

class Report implements Printable, Upper {
    String cons;
    
    Report(String cons) {
        this.cons = cons;
    }
    public String getContents() {
        return cons;
    }
}

class Printer {
    public void printContents(Printable doc) {
        if (doc instanceof Upper) {
            System.out.println((doc.getContents()).toUpperCase());
        }
        else if (doc instanceof Lower) {
            System.out.println((doc.getContents()).toLowerCase());
        }
        else
            System.out.println(doc.getContents());
    }
}

class MarkerInterface {
    public static void main(String[] args) {
        Printer prn = new Printer();
        Report doc = new Report("Simple Funny News~");
        prn.printContents(doc);
    }
}
```

위에서 주목할 것은 toUpperCase 와 toLowerCase

인터페이스 Upper 와 Lower는 클래스에 붙이는 표식으로 사용되었다. Upper는 대문자로 출력하라는 표식이고 Lower는 소문자로 출력하는 표식이다.

### 추상 클래스: Abstract Class

**하나 이상**의 추상 메소드를 갖는 클래스 = '추상 클래스'

```java
public abstract class House {	// 추상 클래스
    public void methodOne() {
        System.out.println("method one");
    }
    @Override
    public abstract void methodTwo();	// 추상 메소드
}
```

몸체가 비어있는 메소드 = 추상 메소드였죠

위 클래스는 하나의 추상 메소드를 갖고 있으니 추상 클래스입니다. 위에서 보이듯이 abstract 키워드를 붙여줘야 한다.

이러한 추상 클래스는 성격이 인터페이스와 유사합니다. 

* 추상 클래스 대상의 인스턴스 생성 불가능
* 다른 클래스에 의해 추상 메소드가 구현되어야 함.
* 어쨌거나 얘는 클래스임. implements가 아니라 extends 키워드 사용해 상속함

다른 클래스들과 마찬가지로 인스턴스 변수, 인스턴스 메소드 등을 갖지만, 추상메소드가 하나라도 있으면 추상 클래스가 된다.