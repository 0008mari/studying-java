# Chapter 24 컬렉션 프레임워크 2

## 24-1 컬렉션 기반 알고리즘

### 정렬

`public static <T extends Comparable<T>> void sort(List<T> list)`

Collections 클래스에 정의되어 있는 제네릭 메소드이다.

인자로 `List<T>`의 인스턴스 전달 가능, 단 T는 `Comparable<T>` 인터페이스를 구현한 상태.

### `<T extends Comparable<? super T>>`

위 sort 메소드의 실제 모습은 다음과 같다. 이렇게 생겼다고 하면 너무 난해해서 한단계 줄여서 소개한 것이다. 

`public static <T extends Comparable<? super T>> void sort(List<T> list)`

매개변수 선언에서 등장하는 `<? super T>`와 문법적 해석은 같으나 목적에서 차이가 있다.

>  설명
>
> Car를 상속하는 클래스인 ECar를 만들었다.
>
> Car는 `Comparable<Car>`를 구현한다.
>
> ECar는 `Comparable<Car>`를 간접 구현한다. (상속에 의해)
>
> ECar는 `Comparable<ECar>`를 구현하지 않는다.
>
> 이런 상황에서도 Comparable이 가능하다 보고 sort를 사용할 수 있게 하고 싶다.
>
> 따라서 Comparable의 조건을 `<T>`가 아니라 `<? super T>`로 설정한다.



### 정렬: `Comparator<T>` 기반

`public static <T> void sort(List<T> list, Comparator<? super T> c)`

이번에는 매개변수 선언에 `<? super T>`가 있다.

따라서 <u>매개변수 c를 대상으로는 T형 인스턴스를 넣는(전달하는) 메소드 호출만 가능</u>하다.

또한 이 매개변수 선언의 의미에는 앞서 설명한 내용도 함께 포함된다. 

> 설명
>
> 앞서 사용한 예시 다시 가져옴.
>
> Car의 정렬을 위해 `Comparator<Car>`를 구현하는 클래스 CarComp와,
>
> 클래스 내부에 compare 메소드를 정의하였다.
>
> 이 메소드를 ECar를 정렬할 때도 사용할 수 있다.
>
> sort의 원형에서 두번째 인자가 `Comparator<? super T>`이기 때문이다.



### 찾기

`public static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`

* list에서 key를 찾아 그 인덱스 값 반환. 못 찾으면 음의 정수 반환.

바이너리 서치 호출에 앞서 정렬 과정이 선행되어야 한다. 정렬되지 않은 상태에서 binarySearch 메소드를 호출하면 정상적인 결과를 얻지 못한다.



### 찾기: `Comparator <T>` 기반

정렬 탐색의 기준을 정의할 수 있다.

```java
class StrComp implements Comparator<String> {
    @Override
    public int compare(String s1, String s2) {
        return s1.compareToIgnoreCase(s2);
    }
}
```

문자열 탐색 시 대소문자를 무시하고 비교하도록 구현하였다.



### 복사하기

`public static <T> void copy(List<? super T> dest, List<? extends T> src)`

* src의 내용을 dest로 복사

dest에 전달되는 컬렉션 인스턴스의 저장 공간이 src에 전달되는 컬렉션 인스턴스의 저장 공간보다 작은 경우 예외 발생. (공간 자동으로 늘리기 x)

아직 컬렉션 인스턴스를 생성하지 않은 상태에서 복사본 만들기는 다음을 참고.

```java
List<String> dest = new ArrayList<>(src);
```



컬렉션 인스턴스에 저장된 내용 전부 출력

System.out.println(dest);