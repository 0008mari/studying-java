# Chapter 7 클래스와 인스턴스

## 07-1 클래스의 정의와 인스턴스의 생성

* 클래스 = 데이터 + 메쏘드

* 인스턴스 변수 = 멤버 변수 = 필드 fields

* 참조변수는 인스턴스를 참조한다.

  ``myAct1 = new BankAccount;``

* 참조변수 관계 끊기: null 대입

## 07-2 생성자와 String 클래스

### String 클래스

문자열 처리를 위해 자바에서 제공하는 클래스.

### 생성자 Constructor

* 생성자의 이름은 클래스의 이름과 동일하다.
* 생성자는 리턴 x, 리턴형 x ---> void

클래스 내에 클래스 이름과 같은 메쏘드 선언하면 그게 생성자다 

클래스 생성 시 괄호 안에 들어가는 값이 생성자의 매개변수로 전달된다.

인스턴스 생성의 마지막 단계.

* 디폴트 생성자: 생성자가 없는 경우 컴파일러에서 만들어줌

## 07-3 자바의 이름 규칙

### 클래스

* 클래스 이름의 첫 문자는 대문자.
* 단어 연결 시 단어 첫 문자는 대문자 

### 메소드, 변수

* 첫 문자는 소문자
* 이어지는 단어 첫 문자는 대문자 

### 상수

* 모든 문자를 대문자
* 단어 연결 시 언더바 (_) 사용 

