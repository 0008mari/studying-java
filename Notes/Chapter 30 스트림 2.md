# Chapter 30 스트림 2

챕29의 내용에 이어서.

이번 챕터는 리퍼런스 역할도 한다. 

## 30-1 스트림의 생성과 연결

### 스트림의 생성: 스트림 생성에 필요한 데이터를 직접 전달

`Stream<T>` 인터페이스에 정의된 static 메소드들

`static<T> Stream<T> of(T t)`

`static <T> Stream<T> of(T...values)`

예제

```java
import java.util.List;
import java.util.Arrays;
import java.util.stream.Stream;

class StreamOfStream {
    public static void main(String[] args) {
        // ex 1
        Stream.of(11, 22, 33, 44)
            .forEach(n -> System.out.print(n + "\t"));
        System.out.println();

        // ex 2
        Stream.of("So Simple")
            .forEach(s -> System.out.print(s + "\t"));
        System.out.println();

        // ex 3
        List<String> sl = Arrays.asList("Toy", "Robot", "Box");
        Stream.of(sl)
            .forEach(w -> System.out.print(w + "\t"));
        System.out.println();       
    }
}
```

```
출력결과
11		22		33		44
So Simple
[Toy, Robot, Box]
```

첫번째 - Integer형으로 오토 박싱

두번째 - 하나의 문자열로 이루어진 스트림

세번째 - 세 개의 문자열로 이루어진 스트림? 아니다!!! 컬렉션 인스턴스 자체로 스트림이다. 

> Stream.of 메소드에 컬렉션 인스턴스를 전달하면 해당 인스턴스 하나로 이루어진 스트림이 생성된다.
>
> 그러나 배열을 전달하는 경우에는 배열에 저장된 요소로 이루어진 스트림이 생성된다.

### DoubleStream, IntStream, LongStream

인터페이스 `Stream<T>`의 타입 매개변수 T에는 int와 같은 기본 자료형의 이름이 올 수 없다.

따라서 DoubleStream과 같은 인터페이스들이 정의되어 있다.

앞서 소개한 of 메소드는 위의 인터페이스에 이렇게 정의되어 있다.

`static DoubleStream of(double...values)`

`static DoubleStream of(double t)`

IntStream, LongStream도 있어요

따라서 위의 메소드를 통해 스트림을 생성하면, (기본 자료형 데이터로 이루어진 경우) 불필요한 오토 박싱, 언박싱을 피할 수 있다.

아래 메소드들은 int, long을 대상으로 범위 내에 있는 값들로 스트림을 구성하는 메소드이다.

range

rangeClosed 

### 병렬 스트림으로 변경

이미 존재하는 스트림을 기반으로 병렬 스트림 생성.

`Stream<T> parallel()`

참고로 parallel 메소드는 BaseStream 인터페이스의 추상메소드이다. BaseStream 인터페이스를 상속하는 것들이 스트림을 참조할 수 있는... `Stream<T>`, `IntStream` 같은 인터페이스들이다.

### 스트림의 연결

`concat(a, b)`



## 30-2 스트림의 중간 연산

아래 내용은 어느정도 레퍼런스 성격이 있기 때문에 키워드만 적어 둔다. 필요 시 찾아보면 될듯.

### 맵핑에 대한 추가 정리

map

flatMap

### 정렬

sorted()

* compareTo, compare() 구현으로 기준 설정 가능

### Looping

스트림을 이루는 모든 데이터 각각을 대상으로 특정 연산을 진행하는 행위.

대표적 루핑 연산으로 forEach(최종연산)가 있다. 

중간 연산에도 루핑 연산 존재.

peek()

중간연산이라는 거 빼고는 forEach와 동일.

## 30-3 스트림의 최종 연산

### sum, count, average, min, max

산술 연산이라 IntStream, DoubleStream, LongStream 대상으로 유효함.

주의: 스트림은 최종 연산을 하는 순간 더 이상의 연산을 진행할 수 없다.

### forEach

forEach

### allMatch, anyMatch, noneMatch

스트림 안에 저장된 데이터가 모두 []인가?

데이터 중에서 []가 하나라도 있는가?

모두 []가 아닌가?

### collect

### 병렬 스트림에서의 collect

병렬 처리가 항상 속도 향상을 부르지는 않는다. 병렬 처리를 위한 과정이 포함되기 때문이다. 따라서 테스트를 통해 병렬 처리의 적합성을 판단해야 한다. 다음 챕터에서 이 적합성 판단 하는 방법 나옴! 

