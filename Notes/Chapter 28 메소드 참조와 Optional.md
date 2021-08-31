# Chapter 28 메소드 참조와 Optional

람다와 람다식과 관련 있는 내용

잘 활용까지는 아니더라도 이해할 수 있는 수준이 되어야 함.

## 28-1 메소드 참조

람다식은 결국 메소드의 정의다.

따라서 이미 정의되어 있는 메소드가 있다면, 이 메소드의 정의가 람다식을 대신할 수 있다.

자바8의 메소드 참조를 통해서.

### 메소드 참조의 4가지 유형과 메소드 참조의 장점

4가지 유형

* static 메소드의 참조

* 인스턴스 메소드의 참조1

* 인스턴스 메소드의 참조2

* 생성자 참조

메소드 참조의 장점

과거와는 달리 코드 자체에 대한 가독성이 아니라 코드의 양을 줄이는 데에 초점이 맞춰진다. 코드의 양이 줄어들면 가독성 개선으로 이어진다는 논리이다.

메소드 참조를 이용하면 람다식으로 줄어든 코드의 양을 조금 더 줄일 수 있다.

### static 메소드의 참조

```java
import java.util.List;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.Collections;
import java.util.function.Consumer;

class ArrangeList {
    public static void main(String[] args) {
        List<Integer> ls = Arrays.asList(1, 3, 5, 7, 9);
        ls = new ArrayList<>(ls);

        Consumer<List<Integer>> c = l -> Collections.reverse(l);
        c.accept(ls);
        System.out.println(ls);
    }
}
```

위의 `reverse` 메소드는 Collections 클래스에 정의된 메소드이다. `List<?>` 자료형의 인스턴스를 인자로 받아 저장 순서를 뒤집는다.

위 예제를 메소드 참조 기반으로 변경하면 다음과 같다.

` Consumer<List<Integer>> c = Collections::reverse;` 

`::` 는 메소드 참조를 의미하는 연산자이다. `클래스명::static메소드명` 

인자 전달 정보를 생략할 수 있는 것은 accept 메소드 호출 시 전달되는 인자를 그대로 전달하기 때문이다. 

### 인스턴스 메소드의 참조1: 인스턴스가 존재하는 상황에서 참조

static과 마차낙지로 인스턴스 메소드도 참조할 수 있다.

```java
import java.util.List;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.Collections;
import java.util.function.Consumer;

class JustSort {
    public void sort(List<?> lst) {    // 인스턴스 메소드
        Collections.reverse(lst);
    }
}

class ArrangeList3 {
    public static void main(String[] args) {
        List<Integer> ls = Arrays.asList(1, 3, 5, 7, 9);
        ls = new ArrayList<>(ls);
        JustSort js = new JustSort();

        Consumer<List<Integer>> c = e -> js.sort(e);
        c.accept(ls);
        System.out.println(ls);
    }
}
```



```java
JustSort js = new JustSort();
Consumer<List<Integer>> c = e -> js.sort(e);
```

람다식에서 같은 지역 내에 선언된 참조변수 js를 접근하고 있다. 람다식에서 접근 가능한 참조변수는 final로 선언되었거나 effectively final이어야 한다. (effectively - 사실상 final과 마찬가지임. 더이상 손 안 댐)

js가 만약에 다른 인스턴스를 참조하게 되거나 그러면 람다식에서 접근할 수 없다.

아니면 람다식 이후의 statement에서 참조 js=null; 하여도 오류다.



```java
JustSort js = new JustSort();
Consumer<List<Integer>> c = js::sort;
```

레퍼런스네임::인스턴스메소드네임

이렇게 하면 된다.



추가예제

```java
import java.util.List;
import java.util.Arrays;

class ForEachDemo {

    public static void main(String[] args) {
        List<String> ls = Arrays.asList("Box", "Robot");
        
        // 람다식 기반
        ls.forEach(s -> System.out.println(s));
        // 메소드 참조 기반
        ls.forEach(System.out::println);
    }
}
```

`Collection <E>` 인터페이스는 `Iterable<T>`를 상속함 그래서 이터러블을 대부분 구현

이터러블에 속한 `forEach` 메소드를 이용한 예제임

각 인스턴스에 대해 `action.accept(t);`를 실행하는데 이 어셉트는 반환x 작업만 하는 추상메소드. 이 추상메소드를 sysout로 덮어씌운것이다앙 

그리고 `System.out`은 `PrintStream` 인스턴스를 참조하는 참조변수이다.

그래서 이렇게 된다.

### 인스턴스 메소드의 참조2

```java
import java.util.function.ToIntBiFunction;

class IBox {
    private int n; 

    public IBox(int i) { n = i; }

    public int larger(IBox b) {
        if(n > b.n)
            return n;
        else
            return b.n;
    }
}

class NoObjectMethodRef {
    public static void main(String[] args) {
        IBox ib1 = new IBox(5);
        IBox ib2 = new IBox(7);

        // 두 상자에 저장된 값 비교하여 더 큰 값 반환
        ToIntBiFunction<IBox, IBox> bf = (b1, b2) -> b1.larger(b2);
        int bigNum = bf.applyAsInt(ib1, ib2);
        System.out.println(bigNum);}
}

// ToIntBiFunction<T, U>	int applyAsInt(T t, U u)
```

람다식

```java
ToIntBiFunction<IBox, IBox> bf = (b1, b2) -> b1.larger(b2);
// 이게

ToIntBiFunction<IBox, IBox> bf = IBox::large;
// 메소드 참조 방식
```

이렇게까지 줄여도 됨 ??

첫번째 전달인자를 대상으로 이 메소드를 호출한다는 약속이 있어서 ㄱㄴ...

### 생성자 참조

```java
interface SMaker {
    String make(char[] ar);
}

class StringMaker {
    public static String chsToString(char[] a, SMaker m) {
        return m.make(a);
    }

    public static void main(String[] args) {
        SMaker sm = (ar) -> {
            return new String(ar); 
            // String 클래스의 생성자 이용
        };
        
        char[] src = {'R', 'o', 'b', 'o', 't'};        
        String str = chsToString(src, sm);
        System.out.println(str);
    }
}
```

람다식을 바꿔보자

```java
// 원본
public static void main(String[] args) {
        SMaker sm = (ar) -> {
            return new String(ar); 
        };
    
// 람다식 줄이기
Function<char[], String> f = ar -> new String(ar);
    
// 메소드 참조 방식으로 변경
Function<char[], String> f = ar -> new String(ar);    
    
// 메소드 참조 방식으로 변경
Function<char[], String> f = String::new;    
```

람다식을 이루는 문장이 단순히 인스턴스의 생성 및 참조 값의 반환일 경우 

`클래스명::new`

이렇게 바꿀 수 있다.

## 28-2 Optional 클래스

반환값이 없을 때 널포인터익셉션!

이거 처리하기 귀찮아! 그래서 단순히 할 수 있도록 옵셔널 도입.

### NullPointerException 예외의 발생 상황

정보가 없는 경우에 null 일 수 있는데, null 예외를 고려하면 단순한 출력문 마저도 복잡해진다. (정보가 여러개라면 더더)

### Optional 클래스의 기본적인 사용 방법

Optional은 멤버 value에 인스턴스를 저장하는 일종의 래퍼(Wrapper) 클래스이다.

```java
if(os1.isPresent())
    Sysout;

// 이거를 이렇게 바꾼다

os1.ifPresent(s -> sysout(s));

```

if문이 사라졌다!

### Optional 클래스로 if~else 대체: map

map 메소드의 소개

```java
import java.util.Optional;

class OptionalMap {
    public static void main(String[] args) {
        Optional<String> os1 = Optional.of("Optional String");
        Optional<String> os2 = os1.map(s -> s.toUpperCase());
        System.out.println(os2.get());

        Optional<String> os3 = os1.map(s -> s.replace(' ', '_'))
                                  .map(s -> s.toLowerCase());
        System.out.println(os3.get());
    }
}
```

map 메소드의 역할: apply 메소드가 반환하는 대상을 Optional 인스턴스에 담아서 반환한다.

### orElse 메소드의 소개

Optional 클래스에는 get 메소드 존재. 유사한 기능의 orElse도 존재한다. 즉 orElse 메소드도 Optional 인스턴스에 저장된 내용물을 반환한다. 단 저장된 내용물이 없을 때, 대신에서 반환할 대상을 지정할 수 있다. (get 과의 차이점. )

```java
import java.util.Optional;

class OptionalOrElse {
    public static void main(String[] args) {
        Optional<String> os1 = Optional.empty();
        Optional<String> os2 = Optional.of("So Basic");

        String s1 = os1.map(s -> s.toString())
                       .orElse("Empty");

        String s2 = os2.map(s -> s.toString())
                       .orElse("Empty");

        System.out.println(s1);
        System.out.println(s2);
    }
}
```

### map과 orElse 사용하여 if~else문 대신하기

대신했습니다^^

### flatMap 메소드

Optional 클래스를 코드 전반에 걸쳐 사용하기도 한다. 그러기 위해서는 map 메소드와 성격이 유사한 flatMap 메소드를 알아야 한다.

먼저 소개

```java
import java.util.Optional;

class OptionalFlatMap {
    public static void main(String[] args) {
        Optional<String> os1 = Optional.of("Optional String");

        Optional<String> os2 = os1.map(s -> s.toUpperCase());
        System.out.println(os2.get());

        Optional<String> os3 = os1.flatMap(s -> Optional.of(s.toLowerCase()));
        System.out.println(os3.get());
    }
}
```

실행결과

`OPTIONAL STRING`

`optional string`

```java
Optional<String> os2 = os1.map(s -> s.toUpperCase());
Optional<String> os3 = os1.flatMap(s -> Optional.of(s.toLowerCase()));
```

차이점!

* map은 람다식이 반환하는 내용물을 알아서 Optional 인스턴스로 감싸준다.
* flatMap은 알아서 x 람다식 내에 Optional 인스턴스로 감싸는 과정이 포함되어야 한다.

flatMap은 Optional 인스턴스를 클래스의 멤버로 두는 경우에 유용하게 사용한다.

```java
import java.util.Optional;

class ContInfo {
    Optional<String> phone;   // null 일 수 있음
    Optional<String> adrs;    // null 일 수 있음

    public ContInfo(Optional<String> ph, Optional<String> ad) {
        phone = ph;
        adrs = ad;
    }
    public Optional<String> getPhone() { return phone; }
    public Optional<String> getAdrs() { return adrs; }
}

class FlatMapElseOptional {
    public static void main(String[] args) {
        Optional<ContInfo> ci = Optional.of(
            new ContInfo(Optional.ofNullable(null), Optional.of("Republic of Korea"))
        );
        
        String phone = ci.flatMap(c -> c.getPhone())
                         .orElse("There is no phone number.");

        String addr = ci.flatMap(c -> c.getAdrs())
                        .orElse("There is no address.");
          
        System.out.println(phone);
        System.out.println(addr);
    }
}
```

만약에 이게 map이었으면

```java
String phone = ci.map(c -> c.getphone()).get()
    			.orElse("There is no phone number");
```

이미 map의 결과로 optional이 감싸져서 .get()으로 꺼내야 한다!!

## 28-3 OptionalXXX

`OptionalInt, OptionalLong, OptionalDouble`

다음 챕터의 Stream을 공부하다 보면 이 클래스들을 접하게 된다.

예제를 중심으로 Optional 과의 차이점을 확인해보자.

