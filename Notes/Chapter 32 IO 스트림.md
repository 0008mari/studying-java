# Chapter 32 I/O 스트림

## 32-1 I/O 스트림에 대한 이해

### IO스트림

IO는 Input Output을 줄인 표현. 데이터의 입출력.

#### 그냥 스트림과 I/O 스트림의 차이

스트림의 주제는 데이터를 어떻게 원하는 형태로 걸러내고 가공할 것인가? 였다.

I/O 스트림의 주제는 데이터를 어떻게 입력, 출력할 것인가? 이다.

예를 들어서 문자열이 여러개 저장된 파일을 불러와 길이가 5이상인 문자열만 출력을 하고자 한다.

* 파일에 저장된 문자열을 꺼내어 컬렉션 인스턴스에 저장: IO스트림
* 컬렉션 인스턴스에 저장된 문자열 중 길이가 5 이상인 문자열만 출력: 스트림

#### IO 스트림 모델

데이터 입출력 대상은 다양하다. 파일, 키보드와 모니터, 프린터 같은 출력장치, 인터넷으로 연결된 서버 등. 자바에서는 입출력 대상에 상관없이 동일한 방법으로 입출력을 진행할 수 있도록 IO스트림 모델이라는 것을 정의하였다. 

I/O 스트림 모델 = I/O 모델

### IO모델과 스트림의 이해, 파일 대상의 입력 스트림 생성

I/O 모델의 스트림

* 입력 스트림
* 출력 스트림

스트림 = 데이터의 흐름.

![](https://t1.daumcdn.net/cfile/tistory/9930B64D5BEC147A2E)

(예제, 이후 예제로 대체)



### 입출력 스트림 관련 코드의 개선

앞서 본 예제의 경우 스트림을 close() 메소드로 종료시켰다. 그런데 입출력 스트림 생성 과정에서 예외가 발생하여 스트림 생성이 실패할 수 있다. 이런 경우 close() 메소드를 호출하는 것이 또다른 예외로 이어질 수 있다. 따라서 다음과 같이 수정하는 것이 안정적이다.

예제 Read2, Write2

위 예제는 try-catch 문을 이용하였음. 아래처럼 try-with-resource 문을 적용하는 것이 가장 간결.

```java
import java.io.InputStream;
import java.io.FileInputStream;
import java.io.IOException;

class Read7FromFile3 {
    public static void main(String[] args) {         
        try(InputStream in = new FileInputStream("data.dat")) {
            int dat = in.read();
            System.out.println(dat);
        } 
        catch(IOException e) {
            e.printStackTrace();       
        }      
    }
}
```

```java
import java.io.OutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

class Write7ToFile3 {
    public static void main(String[] args) {
        try(OutputStream out = new FileOutputStream("data.dat")) {
            out.write(7);
        }
        catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```



### 바이트 단위 입출력 스트림

바이트 단위로 데이터를 입력 및 출력하는 스트림을 가리켜 바이트 스트림이라 한다.

예제 - 바이트 스트림을 생성해서 파일을 복사하는 코드

```java
import java.util.Scanner;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

class BytesFileCopier {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("대상 파일: ");
        String src = sc.nextLine();

        System.out.print("사본 이름: ");
        String dst = sc.nextLine();

        try(InputStream in = new FileInputStream(src) ;
            OutputStream out = new FileOutputStream(dst)) {

            int data;
            while(true) {
                data = in.read();     `// 파일로부터 1바이트를 읽는다.        
                if(data == -1)		// 더 이상 읽어들일 데이터가 없다면
                    break;             
                out.write(data);		// 파일에 1바이트를 쓴다.
            }
        }
        catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```



파일 복사를 위해서는 입력 출력 스트림 모두 필요.

read() 이 메소드는 파일로부터 1바이트 데이터를 읽어들여서, 나머지 3바이트는 0으로 채워서 4바이트의 int형 데이터로 반환한다. 스트림의 끝에 도달해서 더 이상 읽어들일게 없으면 -1 반환.

write() 이것도 위 메소드와 유사하게 4바이트 int형 데이터를 받아 첫번째 바이트만 파일에 저장한다.

파일의 크기에 상관 없이 1바이트씩 복사해서 느리다.

### 보다 빠른 속도의 파일 복사 프로그램

byte 배열을 생성해 이를 기반으로 많은 양의 데이터를 한 번에 읽고 쓴다.

```java
import java.util.Scanner;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

class BufferedFileCopier {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("대상 파일: ");
        String src = sc.nextLine();

        System.out.print("사본 이름: ");
        String dst = sc.nextLine();

        try(InputStream in = new FileInputStream(src) ;
            OutputStream out = new FileOutputStream(dst)) {
            byte buf[] = new byte[1024];
            int len;

            while(true) {
                len = in.read(buf);             
                if(len == -1)
                    break;             
                out.write(buf, 0, len);
            }
        }
        catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```

read는 파일에 저장된 데이터를 전달된 배열에 저장. 읽어 들인 바이트의 수를 반환.

write는 b로 전달된 배열의 데이터를 인덱스 off에서부터 len만큼 파일에 저장. 

위의 예제의 경우 1024바이트, 즉 1킬로바이트 단위로 데이터를 읽고 쓰게 되어 복사 속도가 빨라진다.

## 32-2 필터 스트림의 이해와 활용

입출력 대상과의 연결을 위한 클래스 `FileInputStream`, `FileOutputStream`

이들 외에 기능을 보조하는 성격의 스트림을 생성하는 클래스도 존재한다.

### 바이트 단위로 데이터를 읽고 쓸 줄은 알지만?

파일로부터 단순히 4바이트의 데이터를 읽어들이는 것과 int형 데이터를 읽어들이는 것은 다르다.

int형 데이터를 읽어들이기 위해서는

1. 파일로부터 1바이트 데이터 4개를 읽어들인다.
2. 읽어 들인 데이터 4개를 하나의 int형 데이터로 조합한다.

2단계를 수행하는 것이 '필터 스트림'이다.

* 필터 입력 스트림: 입력 스트림에 연결되는 필터 스트림
  * DataInputStream
  * 기본 자료형 데이터의 입력을 위한 필터 스트림
* 필터 출력 스트림: 출력 스트림에 연결되는 필터 스트림
  * DataOutputStream

```java
import java.io.IOException;
import java.io.FileOutputStream;
import java.io.DataOutputStream;

class DataFilterOutputStream {
    public static void main(String[] args) {
        try(DataOutputStream out = 
                 new DataOutputStream(new FileOutputStream("data.dat"))) {
            out.writeInt(370);
            out.writeDouble(3.14);
        }
        catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```

예제: DataFilterOutputStream.java

입력 예제도 있음.



### 버퍼링 기능을 제공하는 필터 스트림 

필터 스트림 중 자주 사용하는

* BufferedInputStream  버퍼링 기능을 제공하는 버퍼 입력 스트림
* BufferedOutputStream  버퍼링 기능을 제공하는 버퍼 출력 스트림

```java
import java.util.Scanner;
import java.io.IOException;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;

class BufferedStreamFileCopier {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("대상 파일: ");
        String src = sc.nextLine();

        System.out.print("사본 이름: ");
        String dst = sc.nextLine();

        try(BufferedInputStream in = 
                   new BufferedInputStream(new FileInputStream(src)) ;
            BufferedOutputStream out = 
                   new BufferedOutputStream(new FileOutputStream(dst))) {

            int data;
            while(true) {
                data = in.read();             
                if(data == -1)
                    break;             
                out.write(data);
            }
        }
        catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```

 이 예제는 앞서 본 것 처럼 1바이트씩 읽고 쓴다. 이번에는 입력 및 출력 스트림에 버퍼링 기능을 제공하는 필터 스트림을 연결하였다.

그런데 속도는 위의 1024씩 읽고쓰는 것과 큰 차이가 없다.

버퍼 스트림을 연결하면 입력 스트림으로부터 많은 양의 데이터를 가져다 해당 버퍼를 채운다. 

파일 복사에서 시간이 많이 걸리는 것은 메소드 호출의 빈도수보다, 파일에 접근하는 행위. 버퍼를 이용하면 성능 향상에 도움이 된다.

읽을 때, 쓸 때 모두 저장.

### 버퍼링 기능에 대한 대책, flush 메소드

버퍼링은 기본적으로 성능 향상에 도움을 주지만, 다음과 같은 문제점이 있다. (버퍼 출력 스트림에 해당하는 내용): 버퍼 스트림에 저장된 데이터가 파일에 저장되지 않은 상태에서 컴퓨터가 꺼지면...?

프로그램 상에서는 write 호출을 했는데 실제 파일에는 데이터가 저장되지 않는 일이 발생할 수 있다.

버퍼가 꽉차지 않아도 flush() 메소드를 호출하면 명시적으로 버퍼를 비울 수 있다.

자주 부를 필요 당연히 x 스트림 종료 시 자동으로 비워짐 

### 파일에 기본 자료형 데이터를 저장하고 싶은데, 버퍼링 기능도

버퍼링 기능의 필터 스트림과, 기본 자료형 데이터의 저장을 위한 필터 스트림이 따로.

## 32-3 문자 스트림의 이해와 활용

앞서 설명한 입출력 스트림은 바이트 단위로 입출력이 이뤄진다. 그래서 바이트 스트림이라고 한다. 문자의 특성을 고려하여 입출력이 진행되는 '문자 스트림' 이라는 것이 있다.

### 바이트 스트림과 문자 스트림의 차이

기본적으로 입출력은 입출력 과정에서 데이터의 변화가 없다. 그러나 문자를 입출력할 때에는 약간의 데이터 수정이 필요하다.

왜 필요할까? 문자 인코딩이 환경마다 다르기 때문이다. 자바는 모든 문자를 유니코드 기준으로 표현하고, 윈도우의 경우 코드 페이지 949(CP949) 라는 걸 써서 표현한다. 결론은 한글 윈도우의 문자 표현 방식이 자바의 문자 표현 방식과 다르다는 것이다.

따라서 유니코드로 표현된 문자를 해당 운영체제의 문자 표현 방식으로 바꾸어서 저장해야 한다. (구체적으로 한글 윈도우에서, 자바 프로그램을 돌리고 거기서 문자를 파일에 저장하는 것을 생각해보자. 유니코드로 표현된 문자를 CP949으로 바꿔서 저장한다.)

이러한 과정은 프로그래머가 직접 하기에는 번거롭다... 자바가 알아서 해준다. 문자 스트림을 생성해서 문자를 저장하면 된다.

### FileReader & FileWriter

* 파일 복사? 파일 복사는 파일의 내용을 그대로 복사하므로 바이트 스트림을 통해 복사를 진행한다.
* 자바 프로그램에서 문자 하나를 파일에 저장했다가 다시 읽어들이려 한다. 이때 필요한 스트림은? 자바-자바이므로 바이트 스트림.
* OS에서 만든 텍스트 파일의 내용을 자바 프로그램에서 읽어서 출력? OS->자바의 인코딩 변화가 있으므로 문자 스트림.

### BufferedReader & BufferedWriter

문자 스트림에도 필터 스트림을 연결할 수 있다.

* BufferedReader
* BufferedWriter

## 32-4 IO스트림 기반의 인스턴스 저장

 바이트 스트림을 통해서 인스턴스를 통째로 저장하고 꺼내는 것도 가능하다. 이렇듯 인스턴스를 통째로 저장하는 것을 Object Serialization 객체 직렬화라 한다. 역으로 저장된 인스턴스를 꺼내는 것을 객체 역 직렬화Object Deserialization이라 한다.

### ObjectInputStream & ObjectOutputStream

바이트 기반의 기본 입출력에 연결하여 인스턴스를 통째로 입출력하는 스트림.

* ObjectInputStream 
* ObjectOutputStream

사용은 필터 스트림과 유사하나 필터 스트림으로 구분하지 않는다. 필터 스트림이 상속하는 FilterInputStream, FilterOutputStream 두 클래스를 상속하지 않기 때문이다.

추가

입출력의 대상이 되는 인스턴스의 클래스는 `java.io.Serializable`을 구현해야 한다. Serializable은 마커 인터페이스로, 직렬화 가능함을 표시하는 인터페이스이다. (구현해야할 메소드 없음.)

### transient

인스턴스를 저장하면 인스턴스 변수가 참조하는 인스턴스까지 함께 저장이 된다. 함께 저장이 되려면 해당 인스턴스의 클래스도 Serializable을 구현해야 한다.

만약에 참조변수 s가 참조하는 인스턴스의 저장을 원치 않는다면, 다음과 같이 `transient` 선언한다.

인스턴스 변수 앞에 트랜지언트 붙이면 이 변수가 참조하는 인스턴스는 저장되지 않는다. 복원했을 때 null로 초기화된다.

이러한 트랜지언트 선언은 기본 자료형 변수에도 할 수 있다. 그러면 복원 시 0으로 초기화된다.