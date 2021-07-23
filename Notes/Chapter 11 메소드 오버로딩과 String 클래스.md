# Chapter 11 메소드 오버로딩과 String 클래스

## 11-1 메소드 오버로딩

### 메소드 오버로딩의 조건

메소드를 찾을 때 구분하는 요소가 **메소드의 이름, 메소드의 매개변수 정보**이다. 따라서, 메소드의 이름이 같아도 매개변수 선언이 다르면 호출문의 전달인자를 통해 메소드를 구분할 수 있다. 따라서 매개변수 선언이 다르면 동일한 이름의 메소드 정의를 허용한다. 이를 **메소드 오버로딩**이라고 한다.

매개변수 선언: 매개변수의 수, 또는 매개변수의 type

반환형은 해당하지 않는다. 호출한 메소드가 어떤 메소드인지 결정하는 요소가 아니기 때문이다.

### 애매한 상황 - 자동형변환

```java
class AAA {
    void simple(int p1, int p2) {...}
    void simple(int p1, double p2) {...}
}
```

```java
AAA inst = new AAA();
inst.simple(7, 'K'); //어떤 메소드가 호출될 것인가?
```

int와 char를 인자로 받는 simple 메소드는 정의되어있지 않다. 따라서 'K'를 자동형변환한다. 그런데 'K'는 int로도 변환할 수 있고 double 로도 변환할 수 있다~

이러한 상황에서는 자동형변환 규칙을 적용하되, 가장 가까운 위치에 놓여있는 자료형으로의 형 변환을 우선 시도한다.

![img](https://t1.daumcdn.net/cfile/tistory/99F26D495C7DA96817)

이거 기억나지?? char는 int와 가장 가까우므로, 첫번째 줄의 simple 메소드를 적용하게 된다.

이런 애매한 상황은 만들지 않도록 하자.

### 생성자도 오버로딩의 대상이 됩니다.

```java
class Person {
	private int regiNum; //주민번호
    private int passNum; //여권번호
    
    Person(int rnum, int pnum) {
        reginum = rnum;
        passnum = pnum;
    }
    Person(int rnum) {
        reginum = rnum;
        passnum = 0;
    }
    // passnum 정보가 없으면 0으로 초기화한다.
    ...
}
```

네 된다고 하네요. 참 편하겠죠? 예를 들어서 특정 정보가 입력으로 안 들어오면 그건 그냥 두고 초기화할 수도 있는 거죠~

### 키워드 this를 이용한 다른 생성자의 호출

위의 생성자 중 아래거는 이렇게 쓸 수도 있다.

```java
Person(int rnum) {
    this(rnum, 0);
}
```

여기서 this 키워드는 오버로딩 된 다른 생성자를 뜻한다. 즉 또다른 Person(인자가 2개인)을 뜻하며 거기에 rnum이랑 0을 보내서 처리하면 중복된 코드를 줄일 수 있다.

### 키워드 this를 이용한 인스턴스 변수의 접근

```java
class SimpleBox {
    private int data;
    
    SimpleBox(int data) {
        this.data = data;
    }
    void setData(int data) {
        this.data = data;
    }
    int getData() {
        return this.data;
    }
}
```

함수 안에서 이름이 겹치면 그안에서 선언된 애가 우선순위가 높은데

this.data를 이용하여 인스턴스 변수에도 접근할 수 있습니다.

그래서 생성자를 저렇게 만들어도 된다는 거~

## 11-2 String 클래스

### String 클래스의 인스턴스 생성

```java
String str = new String("Simple String"); // 생성
System.out.println(str); // 출력

String str = "Simple String"; // 더 일반적인 생성 방법.

showString("Funny String");
```

마지막줄처럼 하면 일단 "Funny String"에 대한 인스턴스가 만들어지고, 그것의 참조로 showString 함수에 들어간다.

### 문자열 생성을 위한 두가지 방법의 차이점은?

```java
String str = new String("Simple String"); // 방법1
String str = "Simple String"; // 방법2
```

차이가 있다. 아래 예제로 살펴보자

```java
String str1 = "Simple String";
String str2 = "Simple String";

String str3 = new String("Simple String");
String str4 = new String("Simple String");
```

``==`` 연산을 했을 때, 1과 2는 같고, 3과 4는 같지 않다고 나온다.

String 인스턴스는 immutable 인스턴스 이기 때문에, 1과 2 방법의 경우에는 참조만 가능한 인스턴스다. 그래서 문자열 내용이 같은 애들은, 하나의 인스턴스로 참조를 공유하는 방식으로 처리한다.

### String 인스턴스를 이용한 switch 문의 구성

자바 7부터 지원, switch(str) 이렇게 쓰고 밑에 case "one" 이렇게 구성할 수 있다.일반적인 구성은 아닌데 이런 것도 있다고...

## 11-3 String 클래스의 메소드

문자열 처리를 위한 메소드들이다.

### JDK 문서를 참고해야 합니다.

https://docs.oracle.com/javase/8/docs/api/java/lang/String.html

### concat

``public String concat(String str)``

String1.concat(String2); //String1에 String2 를 연결.

새 문자열은 새로 만든 String 인스턴스의 참조로 반환된다.

String1, 2 자리에 큰따옴표 문자열을 넣어도 무방

### substring 문자열추출

``public String subString(int beginIndex)``

beginIndex에서 시작해서 끝까지 추출한다. 인덱스는 문자 첫번째부터 0부터 시작한다~ 그리고 인자 하나 더 넣으면 endIndex 된다. 이때 endIndex는 포함하지 않는다.

즉, str.subString(2, 4); 이렇게 하면 인덱스 2번, 3번에 해당하는 문자열이 반환된다.

### compare

``public boolean equals(Object anObject)``

str.equals("my house");

같으면 true, 다르면 false 반환

Object 라고 써있는 데 String 참조값 전달 가능 (자세한 사항은 상속 파트에서~)



``public int compareTo(String anotherString)``

str1.compare(str2)

두 문자열의 사전 편찬 상 순서. 즉, 글자 단위로 비교한다.

일치하면 0, str1이 앞서면 음수, str2가 앞서면 양수 반환.



``public int compareToIgnoreCase(String str)``

위의 사전순서 비교와 비슷한데, 얘는 대소문자 구분 x



### 기본 자료형의 값을 문자열로 바꾸기

``static String valueOf(...)``

매개변수 자리에서는 boolean, char, double, float, int, long이 올 수 있다.

```java
double e = 2.718281;
String se = String.valueOf(e);
```

호출 방식이 위에랑 다르다. 암튼 이렇게 하면 된다.



### 문자열을 대상으로 하는 +연산, += 연산

* ``+`` 연산: concat
* ``+=`` 연산: 이것도 concat

기본 자료형 (boolean~long에 해당하는)도 문자열과 ``+`` 연산이 가능하다. 그런데 concat()으로는 바로 쓸 수 없다.

concat()의 매개변수로 넣으려면 String.valueOf(17) 이거를 넣어줘야 한다.

### concat 메소드는 이어서 호출이 가능하다.

```java
String str = "AB".concat("CD").concat("EF");
```

왼쪽부터 된다

### 문자열 결합의 최적화

concat을 여러번 하면서 기본 자료형(문자 말고 숫자 등)을 문자열로 변환하는 과정을 여러번 거치면 비효율적이다. concat이 새로운 문자열을 만들어서 반환하는 과정이기 때문이다.

### StringBuilder 클래스

StringBuilder 클래스는 내부적으로 문자열을 저장하기 위한 메모리 공간을 지닌다. 그리고 이 메모리 공간에 문자 추가/삭제가 가능하다. String과의 큰 차이점! 따라서 수정하면서 유지해야할 문자열이 있다면 이 클래스로 관리하는 것이 효율적이다.

``public StringBuilder append(int i)``

``public StringBuilder delete(int start, int end)``

insert, replace, reverse, substring, toString 등등 

다양한 인자(type) 들어가는 애들 많음.. 특히 어펜드는 웬만한 기본자료형 다 들어감

스트링빌더는 인스턴스 생성 과정에서 메모리 크기 지정 가능. 생성자의 인자로 넘겨주면됨~ 

public StringBuilder(int capacity)

public StringBuilder(String str)

정수를 넘겨주면 그게 문자 개수이고 (문자 개수만큼 메모리 할당)

스트링 넘겨주면 그 문자열 + 16개의 문자 저장할 수 있는 메모리 공간 확보

append, delete, insert의 경우, 리턴하는 것은 변경된 인스턴스의 참조값이다.

### StringBuffer 클래스 

StringBuilder 이전에 같은 용도로 쓰던 클래스. 근데 차이점 잇음

StringBuffer는 쓰레드에 안전하지만, 빌더는 그렇지 않다.

멀티 쓰레드에서 StringBuffer 이용하는데, 느리다는 단점이 잇다. 스레드 신경쓰지 않고 Builder 쓰면 더 빠르다.

