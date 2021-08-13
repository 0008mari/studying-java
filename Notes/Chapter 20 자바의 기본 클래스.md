# Chapter 20 자바의 기본 클래스

## 20-1 Wrapper 클래스

래퍼 클래스는 감싸는 클래스이다. 정수, 실수, 문자와 같은 기본 자료형의 값들을 감싼다.

### 기본 자료형의 값을 감싸는 래퍼 클래스

int, double 같은 기본 자료형의 값들도 인스턴스로 표현해야 하는 상황이 있다.

```java
public static void showData(Object obj) {
    System.out.println(obj);
    // toString 메소드 호출하여 반환되는 문자열을 출력한다.
}
```

위 메소드의 인자는 인스턴스이다. 여기에 정수, 실수 등을 전달해야 하는 경우 래퍼 클래스를 이용한다.

```java
public static void showData(Object obj) {
    System.out.println(obj);
    // toString 메소드 호출하여 반환되는 문자열을 출력한다.
}

public static void main(String[] args) {
    Integer iInst = new Integer(3);  // 정수 3을 감싸는 래퍼 인스턴스 생성
    showData(iInst);
    showData(new Double(7.15));
}
```

래퍼 클래스도 toString 메소드를 오버라이딩 하고 있다. 따라서 정상적으로 출력

기본자료형 이름 + 값 

* new Boolean(true)
* new Character('c')
* new Integer(34)

이런식으로 하면 된다 

### 래퍼 클래스의 두 가지 기능

앞서 값을 인스턴스로 감싸는 기능을 살펴보았다. 다른 하나의 기능은 인스턴스에서 값을 꺼내는 것이다. 

값을 인스턴스로 감싸는 행위를 박싱, 꺼내는 행위를 언박싱이라고 한다.

박싱은 인스턴스의 생성으로 이루어지고, 언박싱은 래퍼 클래스에 정의된 메소드의 호출을 통해서 이루어진다.

```java
int num1 = iObj.intValue();
double num2 = dObj.doubleValue();
```

래퍼 인스턴스들은 담고 있는 값을 수정하지 못한다. 따라서 언박싱하고 연산한 후 다시 박싱해야 한다.

``iObj = new Integer(iObj.intvalue() + 10);``

### Auto Boxing & Auto Unboxing

자바5부터 박싱과 언박싱이 필요한 상황에서 이를 자동으로 처리하기 시작했다.

넹 그래서 위에서 말한 거 까먹고 걍 쓰시면 됩니다

```java
Integer iObj = 10;
Double dObj = 3.14;
// 오토 박싱 진행

int num1 = iObj;
double num2 = DObj;
// 오토 언박싱 진행

iObj++; 
// 오토 박싱, 언박싱 진행
```

헷갈리지 않기) 오브젝트 클래스는 대문자로 시작한다

기본 자료형은 소문자로 시작한다.

오토 박싱, 언박싱이 있기 때문에 Integer형 참조변수 num을 int형 변수 num처럼 쓸 수 있당 

### Number 클래스와 Wrapper 클래스의 static method

#### Number 클래스

앞서 소개한 모든 래퍼 클래스는 다음 클래스를 상속한다.

``java.lang.Number``

그리고 이 클래스에는 다음의 추상 메소드들이 존재한다. (즉 Number도 추상 클래스이다.)

``public abstract int intValue()``

``public abstract long longValue()``

``public abstract double doubleValue()``

즉, 래퍼 인스턴스에 저장된 값을 여러가지 형태로 변환할 수 있다. (정수 인스턴스를 long 자료형으로 언박싱 반환 등)

Double -> int의 경우 소수점 이하 삭제

#### Wrapper 클래스의 static 메소드

숫자 뿐만아니라 문자열 기반으로도 Integer 인스턴스를 생성할 수 있음.

Integer 인스턴스의 static 메소드로 max, min, sum 존재

정수에 대한 2진, 8진, 16진수 표현 결과를 반환하는 클래스 메소드 있음 ``Integer.toBinaryString(12)``

다양하게 있어요 ...자바 문서에서 확인 가능

## 20-2 BigInteger, BigDecimal 클래스

int, double에는 표현 한계가 존재. 

### java.math.BigInteger

long으로도 표현 불가능한 수 표현하기.

``BigInteger big1 = new BigInteger("100000000000000000000");``

또한 사칙연산이 지원된다. 근데 add(), subtract() 이런 별도의 메소드 사용함

BigInteger에 저장된 값이 int나 long 범위 안으로 들어오면 int나 long형으로 값을 얻을 수 있다.

``int num = r1.intValueExact();``

long으로 얻으려면 longValueExact() 임

### java.math.BigDecimal

double 형 데이터에는 오차가 존재한다.

```java
BigDecimal d1 = new BigDecimal("1.6");  // 정상적인 방법
BigDecimal d2 = new BigDecimal(1.6);  // 가능하긴 한데...
```

d1이 정확한 표현 방법이다. 그 이유는? d2와 같이 생성자에 값을 전달하면, 오차가 있는 1.6이 전달되기 때문이다.

BigDecimal 클래스에도 사칙연산 등 다양한 메소드가 정의되어 있다.

## 20-3 Math 클래스와 난수의 생성, 문자열 Token의 구분

### Math 클래스

PI, sqrt, 라디안과 디그리 변환, 삼각함수, 지수로그 등

필요할 때 참고

### 난수 생성

``Random rand = new Random();``

``rand.nextInt()``

이런식으로 nextBoolean, nextInt, nextLong 등등 여러가지 있음

범위도 있음 쓸때 참고하삼

### 시드 기반 난수 생성

new Random(); <- 여기에 들어가는 씨드 값에 따라 난수의 패턴이 정해진다. 자바에서는 이 씨드값이 디폴트인 경우 현재 시간을 밀리초 단위로 계산하여 반환한다. 

setSeed(); 이것도 가능

### 문자열의 Token 구분

예시: "PM:08:45"

이런 문자열에서 PM, 08, 45 세개의 정보를 분리해내고자 한다.

``public StringTokenizer(String str, String delim)``

delimeter는 여러개의 문자 가능. "+-/= " 이렇게 전달하면 총 5개임

```java
while(st2.hasMoreTokens())
    System.out.print(st2.nextToken() + ' ');
```

만약에 딜리미터도 토큰으로 반환하고 싶으면 위의 StringTokenizer에서 세번째 인자를 true로 넣으면 된다.

## 20-4 Arrays 클래스

### 배열의 복사

``public static int[] copyOf(int[] original, int newLength)``

copyOfRange(int[] original, int from, int to) 범위 지정 가능

시작인덱스 포함, 끝 인덱스 미포함

위에 두개는 새 배열을 리턴하는거고 그냥 있는데에 하고 싶은 경우도 있다

arraycopy() 이거 있음

### 배열의 비교

``public static boolean equals(int[] a, int[] a2)``

배열에 저장된 데이터 수, 순서, 내용이 같을 때 true

오브젝트에 대하여: Object 클래스에 equals 메소드를 적절하게 오버라이딩 하여 오브젝트 배열에 대해서도 비교할 수 있다.

### 배열의 정렬

``public static void sort(int[] a)``

오름차순.

인스턴스에 대하여: Comparable 인터페이스 구현 필요. 그 중 compareTo 메소드 구현. 

인자로 전달된 o가 작다면 양의정수 / 크다면 음의 정수/ 같다면 0 반환

이렇게 구현하면 된다. 따라서 구현 전에 크고 작음에 대한 판단 기준을 결정해야 한다.

### 배열의 탐색

``public static int binarySearch(int[] a, int key)``

key 가 존재하면 그것의 인덱스값, 없으면 음수 반환

바이너리서치라서 호출 전에 정렬을 해 줘야 된다.

인스턴스에 대하여: compareTo의 구현 내용을 토대로 탐색이 진행된다. 