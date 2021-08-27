# Chapter 27 람다 표현식

## 27-1 람다와 함수형 인터페이스

### 기능 하나를 정의해서 전달해야 하는 상황: 람다

자바는 객체지향 언어라서 코드 흐름의 대부분에 클래스와 인스턴스가 존재한다. 그런데 기능 하나를 정의해 전달해야 하는 상황을 자주 마주친다.

다양한 람다식의 표현을 살펴보자.

### 매개변수가 있고 반환하지 않는 람다식

```java
interface Printable {
    void print(String s);    // 매개변수 하나, 반환형 void
}

class OneParamNoReturn {
    public static void main(String[] args) {
        Printable p;

        p = (String s) -> {System.out.println(s);};    
        // 줄임 없는 표현
        p.print("Lambda exp one.");

        p = (String s) -> System.out.println(s);    
        // 중괄호 생략
        p.print("Lambda exp two.");
        
        p = (s) -> System.out.println(s);    
        // 매개변수 형 생략
        p.print("Lambda exp three.");

        p = s -> System.out.println(s);    
        // 매개변수 소괄호 생략
        p.print("Lambda exp four.");
    }
}
```

줄임이 없음

-> 중괄호의 생략 (메소드의 몸체가 하나의 문장으로만 이루어져 있다.)

* 단, 중괄호 생략 시 문장 끝의 세미콜론도 삭제. (return문은 예외- 별도 예시)

-> 매개변수의 자료형 생략 (해당 람다식이 채우게 될 메소드 정보를 통해 유추 가능)

-> 매개변수가 1개라서 소괄호 생략 가능

```java
interface Calculate {
    void cal(int a, int b);    // 매개변수 둘, 반환형 void
}

class TwoParamNoReturn {
    public static void main(String[] args) {
        Calculate c;
        c = (a, b) -> System.out.println(a + b);
        c.cal(4, 3);

        c = (a, b) -> System.out.println(a - b);
        c.cal(4, 3);

        c = (a, b) -> System.out.println(a * b);
        c.cal(4, 3);
    }
}
```

### 매개변수가 있고 반환하는 람다식

```java
interface Calculate {
    int cal(int a, int b);
}

class TwoParamAndReturn {
    public static void main(String[] args) {
        Calculate c;
        c = (a, b) -> { return a + b; };
        System.out.println(c.cal(4, 3));
        
        c = (a, b) -> a + b;
        System.out.println(c.cal(4, 3));
    }
}
```



`        c = (a, b) -> { return a + b; };` 리턴문의 경우 중괄호 생략 불가.

`        c = (a, b) -> a + b;` 그러나 위의 식은 이렇게 대체 가능. 메소드 몸체의 연산 결과로 반환되는 값이 자동으로 반환 대상이 됨. 보통 이렇게 씀.

### 매개변수가 없는 람다식

매개변수를 표현하는 소괄호 안을 비우면 된다. 

```java
import java.util.Random;

interface Generator {
    int rand();    // 매개변수 없는 메소드
}

class NoParamAndReturn {
    public static void main(String[] args) {
        Generator gen = () -> {
            Random rand = new Random();
            return rand.nextInt(50);
        };

        System.out.println(gen.rand());
    }
}
```

### 함수형 인터페이스와 어노테이션

앞서 보인 람다식 관련 예제에는 다음의 공통점이 하나 있다.

<u>예제에 정의되어 있는 인터페이스에는 추상 메소드가 딱 하나만 존재한다.</u>

이러한 인터페이스를 가리켜 함수형 인터페이스라 한다. 그리고 람다식은 이러한 함수형 인터페이스를 기반으로만 작성될 수 있다.

`@FunctionalInterface`는 이러한 함수형 인터페이스에 부합하는지 확인해주는 어노테이션이다.

static, default 선언이 붙은 메소드의 정의는 함수형 인터페이스 정의에 영향 x

### 람다식과 제네릭

인터페이스는 제네릭으로 정의하는 것이 가능하다. 제네릭으로 정의된 함수형 인터페이스의 예시.

```java
@FunctionalInterface
interface Calculate <T> {
    T cal(T a, T b);
}

class LambdaGeneric {
    public static void main(String[] args) {
        Calculate<Integer> ci = (a, b) -> a + b;
        System.out.println(ci.cal(4, 3));

        Calculate<Double> cd = (a, b) -> a + b;
        System.out.println(cd.cal(4.32, 3.45));
    }
}
```

첫번째 꺼는 정수형 덧셈을 하는 인터페이스의 생성. 두번째 꺼는 실수형 덧셈을 하는 인터페이스의 생성으로 이어진다. 

## 27-2 정의되어 있는 함수형 인터페이스

### 미리 정의되어 있는 함수형 인터페이스 

다음은 `Collection<E>` 인터페이스의 디폴트 메소드인 removeIf이다.

`default boolean removeIf(Predicate<? super E> filter)`

Predicate가 무엇인지? 이렇게 생김.

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

이러한 표준 인터페이스들을 알아야 removeIf와 같은 메소드를 사용할 수 있다.

#### 대표적인 표준 함수형 인터페이스

함수형 인터페이스, 그 안에 위치한 추상 메소드

`Predicate<T>`, `boolean test(T t)`

`Supplier<T>`, `T get()`

`Consumer<T>`, `void accept(T t)`

`Function<T, R>`, `R apply(T t)`

### `Predicate<T>`

`boolean test(T t)`

전달된 인자를 판단하여 true false를 반환해야 하는 상황.

#### 파생

T를 기본 자료형으로 결정하여 정의한 인터페이스들.

IntPredicate, LongPredicate, DoublePredicate

두 개의 인자를 받는 프리디케이트

`BiPredicate<T, U>`, `boolean test(T t, U u)`

파생보다도 본판을 잘 기억하자.

### `Supplier<T>`

단순히 무언가 반환해야 할 때 유용하게 사용 가능.

```java
import java.util.Random;
import java.util.List;
import java.util.ArrayList;
import java.util.function.Supplier;

class SupplierDemo {
    public static List<Integer> makeIntList(Supplier<Integer> s, int n) {
        List<Integer> list = new ArrayList<>();    
        for(int i = 0; i < n; i++)
            list.add(s.get());
        return list;
    }
    
    public static void main(String[] args) {
        Supplier<Integer> spr = () -> {
            Random rand = new Random();
            return rand.nextInt(50);
        };

        List<Integer> list = makeIntList(spr, 5);
        System.out.println(list);

        list = makeIntList(spr, 10);
        System.out.println(list);
    }
}
```

#### 파생

IntSupplier, 등 



### `Consumer<T>`

`void accept(T t);`

전달된 인자 기반으로 반환 이외에 다른 결과를 보일 때. (인자는 전달받지만, 반환은 하지 않음.)

예를 들어서 그 안에서 출력만 할 때!

```java
import java.util.function.Consumer;

class ConsumerDemo {
    public static void main(String[] args) {
        Consumer<String> c = s -> System.out.println(s);
        
        c.accept("Pineapple");    // 출력이라는 결과를 보임
        c.accept("Strawberry");
    }
}
```

### `Function<T, R>`

`R apply(T t);`

전달 인자와 반환값이 모두 있는, 가장 흔히 사용할 수 있는 인터페이스.

#### 파생

이름 규칙이 있어서 적절하게 사용하면 된다.

### removeIf 메소드를 사용해 보자.

컬렉션 인스턴스에 저장된 인스턴스를 test의 인자로 전달햇을 때, true 가 반환되는 인스턴스는 모두 삭제한다. 

```java
import java.util.List;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.function.Predicate;

class RemoveIfDemo {
    public static void main(String[] args) {
        List<Integer> ls1 = Arrays.asList(1, -2, 3, -4, 5);
        ls1 = new ArrayList<>(ls1);

        List<Double> ls2 = Arrays.asList(-1.1, 2.2, 3.3, -4.4, 5.5);
        ls2 = new ArrayList<>(ls2);
        
        // 삭제의 조건
        Predicate<Number> p = n -> n.doubleValue() < 0.0;
        // 각 인스턴스에 전달. 
        ls1.removeIf(p);
        ls2.removeIf(p);

        System.out.println(ls1);
        System.out.println(ls2);
    }
}
```

여기서 T는 Number이고, removeIf의 매개변수 선언이 `<? super E>`이기 때문에 `List<Integer>`, `List<Double>` 모두 가능하다.

