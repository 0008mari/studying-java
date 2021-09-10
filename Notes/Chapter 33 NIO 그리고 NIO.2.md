# Chapter 33 NIO 그리고 NIO.2

본 챕터의 내용은 Chapter.32와 관련이 깊다. 챕32에서 설명한 I/O 스트림을 기능, 성능적으로 보강하기 위해 자바 4, 자바 7에서 추가된 패키지를 설명한다.

## 33-1 파일 시스템

챕 32에서는 `java.io` 패키지를 설명하였다. 자바4에서 NIO라고 불리는 API가 `java.nio` 패키지로 묶여서 추가되었다. (New IO) 그 후로 자바7에서 NIO.2라고 불리는 API가 `java.nio.file` 패키지로 묶여서 추가되었다. 

### 기본적인 파일 시스템

* 윈도우는 루트 디렉토리(최상위 디렉토리)를 여러 개 가질 수 있다. (예: `C:\`, `D:\`)
* 리눅스의 루트 디렉토리는 하나이며 `/`로 표시한다.
* 경로에는 절대경로와 상대경로가 있다.
* 절대경로: 루트 디렉토리부터 시작
* 상대경로: 기준이 현재 디렉토리임.

### Paths와 Path 클래스

`java.nio.file.Path`

결함이 있던 java.io 패키지의 File 클래스를 대체하기 위해 등장.

위는 경로를 표현하기 위한 인터페이스. 

```java
import java.nio.file.Path;
import java.nio.file.Paths;

class PathDemo {
    public static void main(String[] args) {
        Path pt1 = Paths.get("C:\\JavaStudy\\PathDemo.java");
        Path pt2 = pt1.getRoot();
        Path pt3 = pt1.getParent();
        Path pt4 = pt1.getFileName();

        System.out.println("Absolute: " + pt1);
        System.out.println("Root: " + pt2);
        System.out.println("Parent: " + pt3);
        System.out.println("File: " + pt4);
    }
}

```

Path는 경로를 표현하는 데에 쓰인다.

`\`는 이스케이프 시퀀스 문자이므로 문자열 내에서 `\\`로 작성하였다.



CurrentDir.java 현재디렉토리 출력하는 

`toAbsoluthPath()`



### 파일 및 디렉토리의 생성과 소멸

```java
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.Files;
import java.io.IOException;

class MakeFileAndDir {
    public static void main(String[] args) throws IOException {
        Path fp = Paths.get("C:\\JavaStudy\\empty.txt");
        fp = Files.createFile(fp);

        Path dp1 = Paths.get("C:\\JavaStudy\\Empty");
        dp1 = Files.createDirectory(dp1);

        Path dp2 = Paths.get("C:\\JavaStudy2\\Empty");
        dp2 = Files.createDirectories(dp2);

        System.out.println("File: " + fp);
        System.out.println("Dir1: " + dp1);
        System.out.println("Dir2: " + dp2);
    }
}
```

파일 또는 디렉토리 정보를 담은 인스턴스

`createFile`

`createDirectory`

파일 또는 디렉토리에 대한 속성과 권한은 운영체제에 따라 차이가 있다. 명시 안 하면 그냥 기본으로.

예외 발생 상황? 

만약에 새로 생성하고자 하는 해당 디렉토리 혹은 파일이 이미 존재한다.

아니면 파일을 특정 경로에 생성해야되는데 그 경로가 없는경로다.

`createFile` `createDirectory` -> 예외발생

`createDirectories` -> 전달된 경로까지의 디렉토리를 모두 생성하기때문에 예외발생 x

### 파일을 대상으로 하는 간단한 입력 및 출력

```java
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.Files;
import java.nio.file.StandardOpenOption;
import java.io.IOException;

class SimpleBinWriteRead {
    public static void main(String[] args) throws IOException {
        Path fp = Paths.get("C:\\JavaStudy\\simple.bin");
        fp = Files.createFile(fp);

        byte buf1[] = {0x13, 0x14, 0x15};
        for(byte b : buf1)
            System.out.print(b + "\t");
        System.out.println();
        
        // 파일에 바이너리 데이터 쓰기
        Files.write(fp, buf1, StandardOpenOption.APPEND);
        
        // 파일로부터 바이너리 데이터 읽기
        byte buf2[] = Files.readAllBytes(fp);
        
        for(byte b : buf2)
            System.out.print(b + "\t");
        System.out.println();
    }
}
```

I/O 스트림 이용하는 방법에 비해 단순 간단. 따라서 입출력할 데이터 양이 적고 성능이 문제되지 않는 경우에 사용하는 방법이다.

`write` 에 전달할 수 있는 옵션

* APPEND 파일의 끝에 데이터 추가
* CREATE 존재하지 않으면 새로 생성
* CREATE_NEW 새 파일 생성. 이미 존재하면 예외 발생
* TRUNCATE_EXISTING 새 파일 생성인데 이미 있으면 덮어쓰기

디폴트옵션: 크리에이트, 익시스팅

문자데이터 입출력 예제 - SimpleTxtWriteRead.java 참고



### 파일 및 디렉토리의 복사와 이동 

copy

move

메소드.

옵션

* REPLACE_EXISTING 이미 파일이 존재한다면 대체
* COPY_ATTRIBUTES 파일의 속성까지 복사.

이동의 의미: Dir1 내부의 파일과 디렉토리를 Dir2로 옮기는 게 아니라, Dir1을 통째로 옮긴 다음 이름을 Dir2로 수정하는 것이다. 이동 완료 후 Dir1은 사라진다.

## 33-2 NIO.2 기반의 I/O 스트림 생성

우리가 알고 있는 I/O 스트림의 생성방법은 new 를 통한 인스턴스의 생성이다. NIO.2의 File 클래스에서는 스트림의 생성을 위한 메소드를 별도로 제공하고 있어, 보다 간단하게 스트림을 생성할 수 있다.

### 바이트 스트림의 생성

```java
InputStream in = new FileInputStream("data.dat");
// 기존 방식

Path fp = Paths.get("data.dat");
InputStream in = Files.newInputStream(fp);
// NIO.2 의 새로운 스트림 생성 방법.
```

생성된 스트림에는 차이가 없다.

### 문자 스트림의 생성

```java
BufferedWriter bw = new BufferedWriter(new FilterWriter("String.txt"));
// 버퍼 스트림이 연결된 문자 스트림의 생성 방법 (기존 방식)

Path fp = Paths.get("String.txt");
BufferedWriter bw = Files.newBufferedWriter(fp);
// 버퍼링 하는 문자 출력 스트림 생성.
```



## 33-3 NIO 기반의 입출력

자바의 입출력은 조금 느릴 수밖에 없는 구조였다. 성능을 개선한 NIO 패키지가 그래서 자바 4에서 추가되었다. NIO 기반의 입출력은 이전에 보인 방법과 많은 차이가 있다. 그래서 다른 입출력 모델로 생각하고 접근할 필요가 있다.

### NIO의 채널과 버퍼

챕 32에서는 스트림을 생성하여 입출력 진행. 그런데 NIO에서는 스트림 대신 채널이라는 것을 생성한다. 

* 스트림도 채널도 데이터의 입출력을 위한 통로가 된다.
* 스트림은 한 방향으로만 데이터가 이동하지만, 채널은 양방향으로 데이터 이동이 가능하다.
* 채널은 반드시 버퍼에 연결해서 사용해야 한다.

스트림은 입력, 출력 스트림이 구분되지만, 채널은 하나의 채널을 대상으로 읽고 쓰는 것이 가능하다.

NIO에서 채널의 버퍼는 필수이다.

`데이터 -> 버퍼 -> 채널 -> 파일`



예제 - NIO 기반의 파일 복사 프로그램

```java
import java.util.Scanner;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.StandardOpenOption;
import java.io.IOException;

class FileCopierVerNIO {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("대상 파일: ");
        Path src = Paths.get(sc.nextLine());
        
        System.out.print("사본 이름: ");
        Path dst = Paths.get(sc.nextLine());

        // 버퍼 생성
        ByteBuffer buf = ByteBuffer.allocate(1024);
 
        // try(두 개의 채널 생성)
        try(FileChannel ifc = 
                FileChannel.open(src, StandardOpenOption.READ); 
            FileChannel ofc = 
                FileChannel.open(dst, StandardOpenOption.WRITE, 
                                      StandardOpenOption.CREATE)) {
            int num;

            while(true) {
                num = ifc.read(buf);    // ifc로부터 버퍼로 읽어 들임
                if(num == -1)
                    break; 
                
                buf.flip();     // 버퍼에서 데이터 보내기 위한 딸깍!
                ofc.write(buf);     // 버퍼에서 ofc로 데이터 전송            
                buf.clear();    // 버퍼 비우기
            }
        }
        catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```

filp - 버퍼의 상태 전환. 데이터를 저장할 수 있는 상태 -> 읽을 수 있는 상태.

### 성능 향상 포인트는 어디에?

효율적인 버퍼링.

이전 IO모델에서는 입, 출력 각각 버퍼 스트림을 연결했어야 했다. 또, 입력버퍼에 저장된 데이터를 출력버퍼로 이동하는 '버퍼 사이의 데이터 이동 과정'을 반드시 거쳐야 했다. 그러나 이러한 작업을 NIO 모델에서는 생략할 수 있다.

하나 더, Non-direct 버퍼를 생성하지 않고 Direct 버퍼를 생성하여 성능의 향상을 기대할 수 있다.

지금까지 본 Non-direct 버퍼들은 가상머신이 생성하고 유지하는 버퍼이다.

`파일 -> 운영체제 버퍼 -> 가상머신 버퍼 -> 실행 중인 자바 프로그램`

direct 버퍼를 이용하면 다음과 같이 줄어든다.

`파일 -> 운영체제 버퍼 -> 실행 중인 자바 프로그램`

그러나 direct 버퍼의 할당과 해제에 드는 시간적 비용이 논다이렉트 버퍼에 비해 다소 높기 때문에, 입출력할 파일의 크기가 크지 않거나, 버퍼를 빈번히 할당, 해제해야하는 상황이면 논다이렉트 버퍼를 이용해서 입출력을 진행하는 것이 빠를 수 있다.

이외에 기본 자료형 별로 버퍼 클래스가 정의되어 있다.

### 파일 랜덤 접근

파일 랜덤 접근이란, 파일에 데이터를 읽을 때 원하는 위치에 쓰거나 읽는 것을 의미한다. 일반적으로는 나열 순서로 읽는다.

120 240 0.94 0.75

--------------------------> 일반적인 순서

그러나 파일 랜덤 접근을 하면 120->0.94->240 이런것도 가능하다.

NIO의 파일 랜덤 접근은 버퍼를 기반으로 한다. 버퍼에 파일의 데이터를 옮겨 놓고, 버퍼에 저장된 데이터를 순서에 상관 없이 원하는 순으로 읽어 들이는 방식이다.

```java
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.StandardOpenOption;
import java.io.IOException;

class FileRandomAccess {
    public static void main(String[] args) {
        Path fp = Paths.get("data.dat");

        // 버퍼 생성
        ByteBuffer wb = ByteBuffer.allocate(1024);

        // 버퍼에 데이터 저장
        wb.putInt(120);
        wb.putInt(240);
        wb.putDouble(0.94);
        wb.putDouble(0.75);

        // 하나의 채널 생성 
        try(FileChannel fc = FileChannel.open(fp, 
                               StandardOpenOption.CREATE, 
                               StandardOpenOption.READ, 
                               StandardOpenOption.WRITE)) {
            // 파일에 쓰기
            wb.flip(); 
            fc.write(wb);   

            // 파일로부터 읽기
            ByteBuffer rb = ByteBuffer.allocate(1024);
            fc.position(0);    // 채널의 포지션을 맨 앞으로 이동
            fc.read(rb);
            
            // 이하 버퍼로부터 데이터 읽기
            rb.flip();
            System.out.println(rb.getInt());
            rb.position(Integer.BYTES * 2);    // 버퍼의 포지션 이동

            System.out.println(rb.getDouble());
            System.out.println(rb.getDouble());

            rb.position(Integer.BYTES);    // 버퍼의 포지션 이동
            System.out.println(rb.getInt()); 
        } catch(IOException e) {
            e.printStackTrace();       
        }
    }
}
```



[참고](https://ddo-o.tistory.com/40)

