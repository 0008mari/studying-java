# Chapter 12 콘솔 입력과 출력

## 12-1 콘솔 출력

### System.out.println & System.out.print

자바의 대표적인 콘솔 메소드.

println과 print의 차이? ln은 행바꿈을 하고 print는 안한다~

참조값이 매개변수로 전달되면 참조대상의 인스턴스를 대상으로 toString 메소드를 호출한다. 그리고 이때 반환되는 문자열을 출력한다.

### 문자열을 조합해서 출력하는 System.out.printf 메소드

스트링 포매팅

C처럼 %d %f %c 이렇게 하고 반점, 끊고 뒤에 집어넣음

printf는 자동 개행 x 

\n 붙여줘야됨

## 12-2 콘솔 입력

### Scanner 클래스

Scanner 클래스는 자바5에서 소개되었다. 그 전에는 이러한 일을 하는 코드가 쉽지 않앗다.. 스캐너 클래스는 java.util 패키지에 속한다. 생성자 중 일부는 다음과 같다.

``Scanner(File source)``

``Scanner(String source)``

``Scanner(InputStream source)``

스캐너 클래스는 생성자로 전달되는 대상으로부터 데이터를 추출하는 기능을 제공한다.

생성자에서 보이는 바와 같이 파일, 스트링인스턴스 등 다양한 대상으로부터 데이터추출이 가능하다.

### Scanner 클래스의 키보드 적용

Scanner sc = new Scanner(System.in);

System.in은 키보드를 의미하는 인스턴스의 참조변수이다.

실행 과정에서 nextInt 메소드가 호출되면 키보드로부터 데이터가 입력될 때까지 프로그램의 실행 대기 상태에 놓인다. 그리고 정수를 입력한 다음에 엔터를 눌러서 키보드의 입력이 끝이 났음을 알리면, 입력된 정수를 읽어 들이고 실행을 이어 나간다.

### Scanner 클래스의 주요 메소드들

``int nextInt()``

``byte nextByte()``

``String nextLine``

String, double, boolean 등

nextLine 관련

스캐너에 일단 받고 줄단위로 받아옴



