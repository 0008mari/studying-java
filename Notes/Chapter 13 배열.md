# Chapter 13 배열

## 13-1 1차원 배열의 이해와 활용

### 1차원 배열의 이해와 활용

**자바는 배열도 인스턴스다**

``int[] ref = new int[5]; // 길이가 5인 int형 1차원 배열의 생성문``

int[] ref -> int형 1차원 배열 인스턴스를 참조할 수 있는 참조변수의 선언.

int[] 형 참조변수는 int형 1차원 배열을 길이에 상관없이 참조할 수 있다.

### 배열을 대상으로 한 값의 저장과 참조

``ar[0] = 7;``

인스턴스를 배열의 자료형으로 하는 경우의 참조 관계~

배열자료형도 '참조형' 이고 배열을 부르는 이름 자체도 참조형임 

**배열 순회**

```java
for(int i=0; i<sr.length; i++)
    cnum += sr[i].length();
	// String 인스턴스의 length 메소드 순차적 호출.
```

### 배열을 생성과 동시에 초기화하기

```java
int[] arr = new int[3]; // basic

int[] arr = new int[3] {1, 2, 3}; // X
// 뒤의 원소 정보로 길이가 계산이 되므로 길이 생략
int[] arr = new int[] {1, 2, 3}; // O
int[] arr = {1, 2, 3}; // O
```

### 참조변수 선언의 두 가지 방법

```
int[] arr;
int arr[];
```

두가지 모두 같은 방법. 첫번째가 더 선호된다

(응용) 

int[] ar = new int[3]; //선호

int ar[] = new int[3];

### 배열의 참조 값과 메소드

배열도 인스턴스이므로, 메소드 호출 시 참조 값의 전달이 가능하다. 

```java
public static void main(String[] args) {
    int[] ar = {1, 2, 3, 4, 5, 6, 7};
    int sum = sumOfAry(ar); // 배열의 참조 값 전달
    ....
}

// 위에서 호출하는 메소드는 이렇게 정의한다
// 형식매개변수의 type이 int[] 이라는걸 볼 수 있다.
static int sumOfAry(int[] ar) {
    int sum = 0;
    for(int i = 0; i < ar.length; i++)
        sum += ar[i];
    return sum;
}
```

함수에 배열 인스턴스의 참조 값이 전달됨.

```java
static int[] makeNewIntAry(int len) {
    int[] ar = new int[len];
    return ar;
}
```

이렇게 리턴형으로 참조값을 사용할 수도 있다.

### 배열의 초기화와 배열의 복사

배열이 생성되면 모든 요소는 0 또는 null 로 초기화된다.

int형은 0... 인스턴스 배열은 null

그런데 int형 배열과 같은 기본 자료형 배열을 0 이외의 값으로 초기화할 때가 있다.

반복문 돌려서도 초기화할 수 있으나 아래처럼 메소드 호출을 통해 원하는 값을 배열에 저장할 수 있다.

``public static void fill(int[] a, int val)``

-> 두번째 인자로 전달된 값으로 배열 초기화

``public static void fill(int[] a, int fromIndex, int toIndex, int val)``

-> 인덱스 fromIndex ~ (toIndex-1)의 범위까지 val의 값으로 배열 초기화

여기서도 끝 인덱스는 안 들어간다.

**배열 복사하기**

``public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)``

-> 복사 원본의 위치: 배열 src의 인덱스 srcPos

-> 복사 대상의 위치: 배열 dest의 인덱스 destPos

-> 복사할 요소의 수: length

### main 메소드의 매개변수 선언

``public static void main(String[] args) {....}``

여기서 main의 매개변수로 전달되는 배열참조형은 뭘까? 어떻게 들어가는걸가?

``C:\JavaStudy>java Simple Coffee Milk Orange``

이렇게 클래스(Simple.class) 부르면서 뒤에 공백으로 구분된 문자열이 들어가면 그게 매개변수의 원소가 된다.

main메소드 안에서 args[1] 이렇게 불러올 수가 있다.

## 13-2 enhanced for문

### enhanced for문의 이해와 활용

배열에 저장된 값 중에서 특정 조건에 해당하는 값을 찾아라. 배열에 저장된 모든 값에 대해 12%씩 그 값을 증가시킨다.

```java
int[] ar = {1, 2, 3, 4, 5};
for(int i=0; i < ar.length; i++) {
    System.out.println(ar[i]);
}

// 위의 for 문을 enhanced for문으로 구성
for (int e: ar) {
    System.out.println(e);
}
```

순회를 매우 쉽게 할 수 있잖아?

**enhanced for문의 구성**

```
for(요소 : 배열) {
	반복할 문장들
}
```

### 인스턴스 배열을 대상으로 하는 enhanced for문 

동일하게 이용

## 13-3 다차원 배열의 이해와 활용

배열 자체에 대한 이론적 내용이므로 생략.