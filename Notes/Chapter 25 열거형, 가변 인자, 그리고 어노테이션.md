# Chapter 25 열거형, 가변 인자, 그리고 어노테이션

내용이 적어서 묶었을 뿐 연관된 내용은 아니다.

## 25-1 열거형

자바5에서 도입된 기능.

### 인터페이스 기반의 상수 정의 (열거형 이전)

```java
interface Scale {
    int DO = 0; int RE = 1; int MI = 2; int FA = 3;
    int SO = 4; int RA = 5; int TI = 6;
}
```

인터페이스 내에서 선언된 변수는 **public, static, final**이 선언된 것으로 간주한다. 이 Scale은 음계를 표현한 상수들로, 상수 값이 아니라 이름이 중요하다. 즉 값이 바뀌어도 이름이 그대로라면 코드에 영향을 주지 않는다. 이 방법에는 문제가 있다.

인터페이스 여러개로 상수 묶음 여러개를 표현할 경우가 문제. 표현하는 값이 같은 경우 이름에 상관 없이 동작하곤 했다.

### 자료형의 부여를 돕는 열거형

```java
enum Scale {
    DO, RE, MI, FA, SO, RA, TI;
}
```

열거 자료형 Scale의 정의. 그 안에 위치한 이름들을 열거형 값(Enumerated Values)라고 한다. 

열거형은 클래스와 성격이 유사하다. 다음과 같은 참조변수의 선언도 가능하다. 단, 이렇게 선언된 참조변수는 해당 열거형 내에 선언된 '열거형 값'만 대입이 가능하다. 또 switch 문으로 사용도 가능하다.

```java
Scale sc = Scale.DO;

switch(sc) {
case DO:
        ...
case RE:
        ...
case MI:
        ...
}
```

기본적으로 열거형 값은 Scale.DO와 같이 표현해야 하나, case문에서는 표현의 간결함을 위해 DO와 같이 열거형 값의 이름만 명시하기로 약속되어 있다.

위에 인터페이스를 이용하여 상수를 정의했던 문제점이 해결된다. 자료형이 존재하게 되므로 자료형이 달라 컴파일 오류가 발생한다.

### 클래스 내에 정의가 가능한 열거형의 정의

다음 Chapter에서는 클래스 내에 클래스를 정의할 수 있음에 대해 설명한다. 이와 마찬가지로 다음과 같이 클래스 내에 열거형을 정의할 수 있다.

```java
class Customer {
    enum Type {
        ONE, TWO
    }
    ...
   	Type type;
    ...
}
```

특정 클래스 내에서만 사용하고자 하는 열거형 값이 있다면, 위와 같이 해당 클래스 내에 열거형을 정의하면 된다.

### 열거형 값의 정체

열거형 값은 해당 자료형의 인스턴스다.

열거형의 생성자를 직접 정의할 수 있는데, private으로 선언해야 한다.



## 25-2 매개변수의 가변 인자 선언

`...`

`public static int hash(Object...values)`

앞서 23챕에서 본 것과 같이, `...` 이 삽입된 메소드의 매개변수 선언을 가리켜 **가변 인자 선언**이라 한다. 

### 메소드의 가변 인자 선언과 호출

가변 인자 선언을 하면 전달되는 인자 수에 제한을 두지 않을 수 있다.

```java
class Varargs {
    public static void showAll(String... vargs) {
        System.out.println("LEN: " + vargs.length);

        for(String s : vargs) 
            System.out.print(s + '\t');
        System.out.println();
    }

    public static void main(String[] args) {
        showAll("Box");
        showAll("Box", "Toy");
        showAll("Box", "Toy", "Apple");
    }
}
```

실행결과: LEN이 1, 2, 3으로 나오고 전달된 스트링 여러개도 제대로 출력됨.

vargs 는 배열을 참조하기 때문에 length에 접근하여 그 길이를 확인할 수 있음.

### 가변 인자 선언에 대한 컴파일러의 처리

가변 인자는 자바 5에서 추가됨. 그 전에는 이렇게 했음

```java
class VarargsBefore {
    public static void showAll(String[] vargs) {
        System.out.println("LEN: " + vargs.length);

        for(String s : vargs) 
            System.out.print(s + '\t');
        System.out.println();
    }

    public static void main(String[] args) {
        showAll(new String[]{"Box"});
        showAll(new String[]{"Box", "Toy"});
        showAll(new String[]{"Box", "Toy", "Apple"});
    }
}
```

실행 결과는 위와 같음. 컴파일러에서 가변 인자 `...`를 알아서 이렇게 바꿔서 처리.

## 25-3 어노테이션

### 어노테이션의 설명 범위

자바 5에서 도입된 어노테이션. 당시 도입된 어노테이션 타입 세 가지는 다음과 같다. (이들을 어노테이션 타입이라 한다. )

`@Override`

`@Deprecated`

`@SuppressWarnings`

이후에도 어노테이션 관련 내용이 다수 추가되었으나 기본 어노테이션 타입에 대한 이해가 우선이다. 더 깊은 이해가 필요하다면 (API 디자인 등) JSR 175, 250 참고.

### `@Override`

상위 클래스의 메소드 오버라이딩 또는 인터페이스에 선언된 추상 메소드의 구현입니다.

### `@Deprecated`

문제의 발생 소지가 있거나 개선된 기능의 다른 것으로 대체되어서 더 이상 필요 없게 되었음을 뜻하는 단어.

아직은 호환성 유지를 위해 존재하지만, 이후에 사라질 수 있는 클래스 혹은 메소드.

이 어노테이션을 사용 시 컴파일은 잘 되나 컴파일러에서 메시지를 띄운다.

컴파일된 코드에 deprecated된 무언가가 있음을  알리는 메시지이다. 컴파일 옵션으로 어느 부분인지도 확인할 수 있다. (-Xlint)

Deprecated 어노테이션으로 표기된 (클래스, 인터페이스, 메소드)가 호출되는 부분과 구현되는 부분에 대해 경고한다.

### `@SuppressWarnings`

앞서 Deprecated 된 메소드에 대해 컴파일 경고가 발생하는 상황을 보았다. 그런데 당분간 이 메소드를 그냥 그대로 써야 하는 상황이 있을 수 있다. `@SuppressWarnings` 어노테이션을 이용하면 컴파일러의 경고를 지울 수 있다. 

```java
interface Viewable {
    @Deprecated
    public void showIt(String str);

    public void brShowIt(String str);
}

class Viewer implements Viewable {
    @Override 
    @SuppressWarnings("deprecation")
    public void showIt(String str) {
        System.out.println(str);
    }

    @Override
    public void brShowIt(String str) {
        System.out.println('[' + str + ']');
    }
};

class AtSuppressWarnings {
    @SuppressWarnings("deprecation")
    public static void main(String[] args) {
        Viewable view = new Viewer();     
        view.showIt("Hello Annotations");
        view.brShowIt("Hello Annotations");
    }
}
```

SuppressWarnings는 이렇게 사용할 수도 있다. 아래 예제를 보자.

```java
class FallThroughWarnings {
 //   @SuppressWarnings("fallthrough")
    public static void main(String[] args) {
        int n = 3;
        
        switch(n) {
        case 1:
            System.out.println(n);            
        case 2:
            System.out.println(n);
        case 3:
            System.out.println(n);
        }
    }
}
```

각 case문에 break가 존재하지 않아 컴파일 시 경고 메시지가 뜬다. 프로그래머가 이것을 의도한 경우 경고 메시지를 띄우지 않기 위해서 서프레스 워닝을 선언한다.

`-Xlint` 옵션을 통해 경고명 확인 (대괄호 안에 들어 있음) -> 해당 경고명을 사용하여 어노테이션 타입 선언. `@SuppressWarnings("경고1", "경고2")`

