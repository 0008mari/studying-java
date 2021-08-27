# Chapter 26 네스티드 클래스와 람다의 소개

## 26-1 네스티드 클래스와 이너 클래스

다음과 같이 클래스 안에 또 다른 클래스를 정의할 수 있다.

```java
class Outer {
    class Nested {...}
}
```

* 네스티드 클래스: 클래스 내에 정의된 클래스
* 감싸는 클래스: 외부(Outer) 클래스

### 네스티드 클래스의 구분

네스티드 클래스는 스태틱 선언 여부를 기준으로 다음과 같이 나눈다.

* Static 네스티드 클래스
* Non-static 네스티드 클래스 (= 이너Inner 클래스)

그리고 이너 클래스는 다시 세 종류로 나뉜다.

* 멤버 이너 클래스
* 로컬 이너 클래스
* 익명 이너 클래스 

### Static 네스티드 클래스

static 특성 반영. 자신을 감싸는 외부 클래스의 인스턴스와 상관 없이 static 네스티드 클래스의 인스턴스 생성이 가능하다.

static 네스티드 클래스 내에서는 외부 클래스에 static으로 선언된 변수와 메소드에만 접근이 가능하다.

### 이너 클래스의 구분

정의된 위치에 따라

* 멤버 클래스
  * 인스턴스 변수, 인스턴스 메소드와 동일한 위치에 정의
* 로컬 클래스
  * 중괄호 내에, 특히 메소드 내에 정의 

```java
class Outer {
    class MemberInner {...} // 멤버 클래스
    
    void method() {
        class LocalInner {...} // 로컬 클래스
    }
}
```

이름 없는 클래스 (위 두가지와 별개)

* 익명 클래스 

### 멤버 클래스

멤버 클래스 내에서 아우터 클래스의 인스턴스 변수에 접근이 가능하다.

즉. 아우터 인스턴스 o1 생성- > o1 기반으로 멤버 인스턴스 생성 o1m1

멤버 클래스의 인스턴스는 외부 클래스의 인스턴스에 종속적이다. 

### 멤버 클래스의 사용

클래스의 정의를 감추어야 할 때 유용하게 사용할 수 있다.

코드의 유연성. 

,p.649

### 로컬 클래스

멤버 클래스와 유사점 있으나, 클래스의 정의 위치가 if문이나 while문 , 메소드 몸체와 같은 블록 내부이다.



### 익명 클래스

클래스의 정의와 인스턴스의 생성을 하나로 묶을 수 있다. 

```java
public Printable getPrinter() {
    return new Printable() {
        public void print() {
            System.out.println(con);
        }
    }
}
```



## 26-2 람다의 소개

자바의 완.전.성.

### 람다의 이해

람다를 사용하면 코드가 짧아지면서 가독성을 높일 수 있다. 

람다와 익명 클래스는 분명 다르나, 둘 다 인스턴스의 생성으로 이어지고, 람다식이 익명 클래스의 정의를 일부 대체하기 때문에 익명 클래스의 정의를 기반으로 람다식을 이해하는 것도 좋은 방법이다. 



```java
// 코드 변화를 보라

class Lambda2 {
    public static void main(String[] args) {
        Printable prn = new Printer() {		// 익명 클래스
            public void print(String s) {
                System.out.println(s);
            }
        };
        
        prn.print("What is Lambda?");
    }
}


class Lambda3 {
    public static void main(String[] args) {
        Printable prn = (s) -> {System.out.println(s); };
        prn.print("What is Lambda?");
    }
}



```

람다식

`        Printable prn = (s) -> {System.out.println(s); };`

이거임. 결과로 인스턴스가 생성됨.

`(s) ` 매개변수임

`->` 람다연산자



### 람다식의 인자 전달

```java
int n = 10;

method(10); // 이때 void method(int n) {...}
 
Prinable prn = (s) -> { System.out.println(s); };
// 이거 가능

// 따라서 다음과 같이 람다식을 메소드의 인자로 전달할 수 있다.
method((s) -> System.out.println(s)); 
// 이때 void method(Printable prn) {...}
```

람다식을 메소드의 인자로 전달할 수 있다.

```java
interface Printable {
    void print(String s);
}

class Lambda4 {
    public static void ShowString(Printable p, String s) {
        p.print(s);
    }

    public static void main(String[] args) {
        ShowString((s) -> { System.out.println(s); }, "What is Lambda?");
    }
}
```

