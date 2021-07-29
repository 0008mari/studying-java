# Chapter 14 클래스의 상속1: 상속의 기본

## 14-1 상속의 기본 문법 이해

### 상속에 대한 매우 치명적인 오해

상속은 코드의 재활용을 위한 문법이다?

아니다!!

"연관된 일련의 클래스들에 대해 공통적인 규약을 정의할 수 있습니다."

이렇게 말하는 것이 모범이지만 지금 이해하기는 어렵다. 챕16까지 공부해보자.

### 상속의 가장 기본적인 속성

단순하게: 기존에 정의된 클래스의 메소드와 변수를 추가하여 새로운 클래스를 정의하는 것.

```java
class Man {
    String name;
    public void tellYourName() {
        System.out.println("My name is " + name);
    }
}
```

```java
class BusinessMan extends Man {
    // Man을 상속하는 BusinessMan
    String company;
    String positionl
        public void tellYourInfo() {
        System.out.println("My company is " + company);
        System.out.println("My position is " + position);
        tellYourName();
        // Man 클래스를 상속했기 때문에 호출 가능! 
    }
}
```



부르는 법

* 상속의 대상이 되는 클래스: 상위 클래스, 기초 클래스, 부모 클래스 (Man)
* 상속을 하는 클래스: 하위 클래스, 유도 클래스, 자식 클래스 (BusinessMan 클래스)

기호

![WawOops: [UML] ClassDiagram (클래스 다이어그램)](http://1.bp.blogspot.com/-Rk-K0wOMeJw/UVuRVUk2TYI/AAAAAAAAAOY/IfGrB1-BFs4/s1600/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2013-04-03+%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB+11.17.18.png)

이렇게 박스로 클래스를 쓰고 화살표로 상속을 표시한다. UML 기호라는 것이다. 

이 화살표는 하위클래스에서 시작해서 상위클래스로 향한다.

### 상속과 생성자

Man, Businessman 클래스에 각각 적절한 생성자를 정의해주었다. 그런데 문제가 있다. BusinessMan의 인스턴스가 생성되면, 그 안에는 Man 클래스의 상속에 의해 다음 변수도 존재하게 되는데 이를 초기화하지 않는 문제가 발생한다.

(위에서 String name; 그런데 BusinessMan의 생성자에서는 이를 초기화하지 않음.)

따라서 다음 예와 같이 BusinessMan의 생성자에서 name도 초기화해주어야한다.

```
public BusinessMan(String name, String company, String position) {
	// 상위 클래스 Man의 멤버 초기화
	this.name = name;
	
	// 클래스 BusinessMan의 멤버 초기화
	this.company = company;
	this.position = position;
}
```



**상속 관계의 적절한 생성자 정의**

하위 클래스의 생성자에서 상위 클래스의 생성자를 명시적으로 호출하지 않으면, (**묵시적 호출**) 인자를 받지 않는 생성자가 자동으로 호출된다.

하위 클래스에서 적절하게 상위 클래스의 생성자를 호출해주어야한다.

-> 키워드 super

```java
class SubCLS extends SuperCLS {
    public SubCLS() {
        System.out.println("Con: SubCLS()");
    }
    public SubCLS(int i) {
        super(i); // 상위 클래스의 생성자를 지정 및 호출
        System.out.println("Con: SubCLS(int i)");
    }
    public SubCLS(int i, int j) {
        super(i, j); // 상위 클래스의 생성자를 지정 및 호출
        System.out.println("Con: SubCLS(int i, int j)");
    }
}
```

super를 이용한 상위 클래스의 생성자 호출은 생성자의 첫문장으로 등장해야 함. 어기면 컴파일 오류.

### 상속 관계에 있는 두 클래스의 적절한 생성자 정의

자바는 상속관계에 있을지라도, 상위 클래스의 멤버는 상위 클래스의 생성자를 통해서 초기화하도록 유도하고 있다.

상속관계에 있을지라도 인스턴스 변수는 각 클래스의 생성자를 통해서 초기화해야 한다!

### 단일 상속만을 지원하는 자바

하나의 클래스가 상속할 수 있는 클래스의 수가 최대 하나. 즉, extends 뒤에 붙일 수 있는 게 하나 뿐.

그러나 상속을 더해가며 깊이를 쌓는 것은 가능.

## 14-2 클래스 변수, 클래스 메소드와 상속

static 선언이 붙는 클래스 변수와 클래스 메소드도 상속이 될까?

### static 선언이 붙는 '클래스 변수'와 클래스 메소드의 상속

클래스 변수와 클래스 메소드는 딱 하나만 존재, 즉 상속의 대상이 아니다.

상위 클래스에 위치한 클래스 변수와 메소드에 하위 클래스에서 어떻게 접근하는가?

-> 답: 변수의 이름만으로 접근이 가능하다. 그러나, 접근 지시자에 따라간다. private 의 경우 상속 관계의 클래스에서 접근 불가능!

