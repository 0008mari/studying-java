# Chapter29 스트림Stream 1

## 29-1 스트림의 이해와 스트림의 생성

### 스트림의 이해

데이터의 흐름을 스트림이라 한다. 그리고 데이터는 중간중간 연산을 거친다. 여러 연산 사이에 순서가 중요하다. 

연산의 종류는 크게 나누자면 아래와 같다.

* 중간 연산: 마지막이 아닌 위치에서 진행이 되어야 하는 연산
* 최종 연산: 마지막에 진행이 되어야 하는 연산.

### 스트림 첫번째 예제

```java
import java.util.Arrays;
import java.util.stream.IntStream;

class MyFirstStream {
    public static void main(String[] args) {
        int[] ar = {1, 2, 3, 4, 5};

        // 스트림 생성        
        IntStream stm1 = Arrays.stream(ar);

        // 중간 파이프 구성
        IntStream stm2 = stm1.filter(n -> n%2 == 1);

        // 최종 파이프 구성
        int sum = stm2.sum();

        System.out.println(sum);
    }
}
```

결과: 9

filter: 홀수만 통과시키는 연산.



### 스트림의 특성

위는 이해를 돕기 위한 것이고 실제로는 아래와 같이 쓴다.

```java
import java.util.Arrays;

class MyFirstStream2 {
    public static void main(String[] args) {
        int[] ar = {1, 2, 3, 4, 5};

        int sum = Arrays.stream(ar)
                        .filter(n -> n%2 == 1)
                        .sum();

        System.out.println(sum);
    }
}
```



스트림의 연산은 효율과 성능을 고려하여 Lazy 처리 방식으로 동작한다. 최종 연산이 생략된다면 중간 연산을 많이 해도 의미가 없다.



### 본서에서 스트림을 설명하는 방향

* 스트림의 생성 방법
* 중간 연산의 종류와 내용
* 최종 연산의 종류와 내용

이번 챕터에서는 전반을 훑어보고 나머지는 다음 챕터에서 설명.

### 스트림 생성하기: 배열

배열에 저장된 데이터를 대상으로 스트림 생성하기.

`public static <T> Stream<T> stream(T[] array)`

Array 클래스에 정의된 함수다.

```java
import java.util.Arrays;
import java.util.stream.Stream;

class StringStream {
    public static void main(String[] args) {
        String[] names = {"YOON", "LEE", "PARK"};
        
        // 스트림 생성
        Stream<String> stm = Arrays.stream(names);
        
        // 최종 연산 진행
        stm.forEach(s -> System.out.println(s));
    }
}
```

결과: YOON \n LEE \n PARK \n



유사 함수: intStream, DoubleStream, LongStream

### 스트림 생성하기: 컬렉션 인스턴스

`default Stream<E> stream()`

컬렉션 인스턴스를 대상으로 ㄱㄴ

```java
import java.util.List;
import java.util.Arrays;

class ListStream {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Toy", "Robot", "Box");
        list.stream()
          .forEach(s -> System.out.print(s + "\t"));
        System.out.println();
    }
}
```



## 29-2 필터링과 맵핑

필터링과 맵칭은 스트림의 중간 연산 중 한 종류이다.

배열과 컬렉션 인스턴스 양 측을 대상으로 생성된 스트림 모두에 적용 가능하다.

### 필터링

`Stream<T> filter(Predicate<? super T> predicate)`

프리디케이트의 다음 추상 메소드의 구현에 해당하는 람다식을 인자로 전달해야 한다.

`Predicate<T>    boolean test(T t)`

true가 반환되면 남기고 false면 거른다 (버린다).

### 맵핑1

맵핑을 진행하면 스트림의 데이터 형이 달라지는 특징이 있다.

예를 들어서 문자열을 문자열 길이(정수형)으로 변환한다.

제네릭 메소드.

```java
import java.util.List;
import java.util.Arrays;

class MapToInt {
    public static void main(String[] args) {
        List<String> ls = Arrays.asList("Box", "Robot", "Simple");
        
        ls.stream()
          .map(s -> s.length())
          .forEach(n -> System.out.print(n + "\t"));

        System.out.println();
    }
}
```

오토 박싱 진행됨

만약에 map 대신 mapToInt하면 오토박싱 안되고 int로 결과물 나옴



### 맵핑2

```java
import java.util.List;
import java.util.ArrayList;

class ToyPriceInfo {
    private String model;
    private int price;
    
    public ToyPriceInfo(String m, int p) {
        model = m;
        price = p;
    }

    public int getPrice() {
        return price;
    }
}

class ToyStream {
    public static void main(String[] args) {
        List<ToyPriceInfo> ls = new ArrayList<>();
        ls.add(new ToyPriceInfo("GUN_LR_45", 200));
        ls.add(new ToyPriceInfo("TEDDY_BEAR_S_014", 350));
        ls.add(new ToyPriceInfo("CAR_TRANSFORM_VER_7719", 550));
                  
        int sum = ls.stream()
                       .filter(p -> p.getPrice() < 500)
                       .mapToInt(t -> t.getPrice())
                       .sum();
        
        System.out.println("sum = " + sum);
    }
}
```

예시를 보면 중간연산 2번 최종연산 1번 한다 

price 가 500 미만인 장난감 가격의 총합. 



## 29-3 리덕션, 병렬 스트림

최종 연산을 대표하는 reduce 메소드.

### 리덕션과 reduce 메소드

리덕션Reduction은 데이터를 축소하는 연산을 뜻한다. 앞서 보인 sum도 리덕션 연산에 해당한다.

조금 다른 리덕션을 활용하는 메소드 `T reduce(T identity, BinaryOperator<T> accumulator)`

전달하는 람다식에 따라 연산의 내용이 결정됨 

accumulator라는건?  data1과 2를 대상으로 이항연산 진행 -> 그 결과를 data3과 연산 -> ... so on 



reduce 메소드는 첫번째 인자로 전달된 값을 스트림이 빈 경우에 반환한다.



### 병렬 스트림 

자바는 언어 차원에서 병렬 처리(하나의 작업을 둘 이상의 작업으로 나누어 진행하는 것)를 지원한다. 프로그래머가 어떻게 동작하는지 몰라도 병렬 처리를 시킬 수 있다, 

`parallelStream` 메소드.

병렬처리의 핵심은 연산의 단계를 줄이는 데에 있다.

