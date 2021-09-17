# Chapter 34 쓰레드 그리고 동기화

자바 언어 차원에서 멀티 쓰레드를 지원한다.

## 34-1 쓰레드의 이해와 쓰레드의 생성

### 쓰레드의 이해와 쓰레드의 생성 방법

쓰레드는 실행중인 프로그램 내에서 <u>또 다른 실행의 흐름을 형성하는 주체</u>를 의미한다. 예를 들어 다음과 같이 프로그램을 실행하면, 가상머신은 하나의 쓰레드를 생성해서 main 메소드의 실행을 담당하게 한다.

```java
class CurrentThreadName {
    public static void main(String[] args) {
        Thread ct = Thread.currentThread();
        String name = ct.getName();
        System.out.println(name);
    }
}
```

출력 결과를 보면 쓰레드의 이름이 main임을 알 수 있다.

### 쓰레드를 생성하는 방법

main 쓰레드 외에 다른 쓰레드를 생성해보겠다. 쓰레드를 추가하면 추가한 수만큼 프로그램 내에서 다른 실행의 흐름이 만들어진다.

```java
class MakeThreadDemo {
    public static void main(String[] args) {
        Runnable task = () -> {
            int n1 = 10;
            int n2 = 20;
            String name = Thread.currentThread().getName();
            System.out.println(name + ": " + (n1 + n2));
        };

        Thread t = new Thread(task);
        // 인스턴스 생성 시 run 메소드의 구현 내용 전달.
        t.start();
        // 쓰레드의 생성 및 실행

        System.out.println("End " + Thread.currentThread().getName());
    }
}
```

쓰레드의 생성을 위해 제일 먼저 할 일은 `java.lang.Runnable` 인터페이스를 구현하는 클래스의 인스턴스를 생성하는 일이다.

`Runnable`은 추상 메소드 `void run()` 단 하나만 존재하는 함수형 인터페이스이다. 따라서 예제에서는 람다식을 기반으로 메소드의 구현과 인스턴스의 생성을 동시에 진행하였다. 이렇게 구현된 메소드는 새로 생성되는 쓰레드에 의해 실행되는 메소드이다.

기본적으로 주어지는 이름에 따라 Thread-0으로 출력된다.

Thread 생성자를 통해 이름을 전달할 수 있다.

위와같이 생성된 쓰레드는 자신의 일을 마치면 자동으로 소멸된다. (소멸 = 쓰레드 생성을 위해 할당했던 모든 자원의 해제.)

쓰레드의 실행 흐름을 조절하거나 예측하는 것은 어렵다. 쓰레드가 처한 상황에 따라서, 또는 운영체제가 코어를 쓰레드에 할당하는 방식에 따라서 실행 속도에 차이가 있기 때문이다. 보통은 쓰레드 하나에 CPU의 코어 하나가 할당되어 동시에 실행이 이루어진다.

> 쓰레드가 코어의 수보다 많이 생성되는 경우
>
> CPU 코어 수가 하나이던 시절의 멀티 쓰레드 프로그래밍에는 다음과 같은 장점이 있었다.
>
> * CPU 코어가 둘 이상인 것과 같은 효과
> * 하나의 코어가 둘 이상의 쓰레드를 담당하므로 코어 활용도 높음.

### 쓰레드를 생성하는 두번째 방법

앞서 설명한 쓰레드의 생성 과정은 다음 세 단계이다.

1. Runnable을 구현한 인스턴스 생성
2. Thread 인스턴스 생성
3. start 메소드 호출

다음의 두단계를 거쳐 쓰레드를 생성할 수도 있다.

1. Thread를 상속하는 클래스의 정의와 인스턴스 생성
2. start 메소드 호출

위의 1,2 단계를 한 단계로 합쳐버렸다.

```java
class Task extends Thread {
    // Thread의 run 메소드 오버라이딩.
    public void run() {
        int n1 = 10;
        int n2 = 20;
        String name = Thread.currentThread().getName();
        System.out.println(name + ": " + (n1 + n2));    
    }
}

class MakeThreadDemo2 {
    public static void main(String[] args) {
        Task t1 = new Task();
        Task t2 = new Task();
        // 오버라이딩된 run 메소드 실행.
        
        t1.start();
        t2.start();

        System.out.println("End " + Thread.currentThread().getName());
    }
}
```



## 34-2 쓰레드의 동기화

앞서 본 2가지 방법 중, Runnable 인터페이스를 구현하는 방식을 기반으로 설명을 이어나가겠다. 람다식을 기반으로 하는 이 방식이 보다 보편적이다.

### 쓰레드의 메모리 접근 방식과 그에 따른 문제점

둘 이상의 쓰레드가 하나의 메모리 공간(하나의 변수)에 접근했을때 발생하는 문제점.

`join`  특정 쓰레드의 실행이 완료되길 기다릴 때 호출하는 메소드.

### 동일한 메모리 공간에 접근하는 것이 왜 문제가 되는가?

한 순간에 한 쓰레드만 변수에 접근하도록 해야 한다.

### 동기화Syncronization 메소드

```java
class Counter { 
    int count = 0; 

    synchronized public void increment() {
        count++;
    }

    synchronized public void decrement() {
        count--;
    }

    public int getCount() { return count; }
}

class MutualAccessSyncMethod {
    public static Counter cnt = new Counter();

    public static void main(String[] args) throws InterruptedException {        
        Runnable task1 = () -> {
            for(int i = 0; i<1000; i++)
                cnt.increment();
        };

        Runnable task2 = () -> {
            for(int i = 0; i<1000; i++)
                cnt.decrement();
        };


        Thread t1 = new Thread(task1);
        Thread t2 = new Thread(task2);
        
        t1.start();
        t2.start();
    
        t1.join();
        t2.join();
     
        System.out.println(cnt.getCount());
    }
}
```



### 동기화Syncronization 블록

메소드 전체에 동기화를 걸지 않고, 문장 단위로 동기화 선언을 하는 것이다.

this의 의미: 동기화 블록 묶어서 하기. (이 인스턴스의 다른 동기화 블록과 더불어 동기화하겠다.)

> Thread-safe의 의미
>
> 클래스별 쓰레드 안전 여부가 존재한다.

## 34-3 쓰레드를 생성하는 더 좋은 방법

### 쓰레드 생성과 활용 방법

쓰레드의 생성과 소멸은 그 자체로 시스템에 부담을 주는 일이다. 따라서 쓰레드 풀이라는 것을 만들고, 그 안에 미리 제한된 수의 쓰레드를 생성해 두고 재활용하는 기술을 사용해 왔다.

쓰레드 풀의 구현은 간단하지 않다. 그러나 자바에서 지원하는 `concurrent` 패키지를 활용하면 직접 구현하지 않고 생성 가능하다.

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

class ExecutorsDemo {
    public static void main(String[] args) {
        Runnable task = () -> {     // 쓰레드에게 시킬 작업
            int n1 = 10;
            int n2 = 20;
            String name = Thread.currentThread().getName();
            System.out.println(name + ": " + (n1 + n2));
        };
        
        ExecutorService exr = Executors.newSingleThreadExecutor();
        exr.submit(task);    // 쓰레드 풀에 작업을 전달

        System.out.println("End " + Thread.currentThread().getName());

        exr.shutdown();    // 쓰레드 풀과 그 안에 있는 쓰레드의 소멸
    }
}
```

쓰레드풀 소멸

shutdown();

### Callable & Future

Runnable 인터페이스를 기반으로 람다식 생성하는 경우, Runnable의 추상메소드 run의 리턴형이 void 이기때문에 작업의 결과를 return으로 반환하는 것이 불가능하다. Runnable 대신 Callable을 사용하면 된다.

```java
@FunctionalInterface 
public interface Callable<V> {
    V call() throws Exception;
}
```

예시

```java
import java.util.concurrent.Future;
import java.util.concurrent.Callable;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.ExecutionException;

class CallableDemo {
    public static void main(String[] args) 
              throws InterruptedException, ExecutionException {
        Callable<Integer> task = () -> {
            int sum = 0;
            for(int i = 0; i < 10; i++)
                sum += i;
            return sum;
        };
        
        ExecutorService exr = Executors.newSingleThreadExecutor();
        Future<Integer> fur = exr.submit(task);
        // 메소드의 반환값을 Funture<V>형 참조변수에 저장.

        
        Integer r = fur.get();
        // 쓰레드의 반환값 획득 
        System.out.println("result: " + r);
        exr.shutdown();
    }
}
```

Callable에 인티져 전달하는거 보이시져

이 경우에는 메소드의 반환값을 저장해야 한다.

### syncronized를 대신하는 ReentrantLock

동기화 블록과 동기화 메소드를 대신할 수 있는 ReentrantLock.

```java
class MyClass {
    ReentrantLock criticObj = new ReentrantLock();
    void myMethod(int arg) {
        criticObj.lock(); // 문을 잠근다.
        .... // 한 쓰레드에 의해서만 실행되는 영역.
        criticObj.unlock(); // 문을 연다.
    }
}
```

위와 같이 작성하면 unlock()까지 대기

그런데 프로그래머의 실수로 unlock();을 빼먹을 수 있으므로 다음과 같이 쓰는 것이 안정적이다.

```java
class MyClass {
    ReentrantLock criticObj = new ReentrantLock();
    void myMethod(int arg) {
        criticObj.lock(); // 문을 잠근다.
       	try {
        .... // 한 쓰레드에 의해서만 실행되는 영역.
        } finally {
        	criticObj.unlock(); // 문을 연다.
        }
    }
}
```

예제



```java
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.TimeUnit;

class Counter { 
    int count = 0;
    ReentrantLock criticObj = new ReentrantLock();

    public void increment() {
        criticObj.lock();

        try {
            count++;
        } finally {
            criticObj.unlock();
        }
    }

    public void decrement() {
        criticObj.lock();

        try {
            count--;
        } finally {
            criticObj.unlock();
        }
    }

    public int getCount() { return count; }
}

class MutualAccessReentrantLock {
    public static Counter cnt = new Counter();

    public static void main(String[] args) throws InterruptedException {        
        Runnable task1 = () -> {
            for(int i = 0; i < 1000; i++)
                cnt.increment();
        };

        Runnable task2 = () -> {
            for(int i = 0; i < 1000; i++)
                cnt.decrement();
        };

        ExecutorService exr = Executors.newFixedThreadPool(2);
        exr.submit(task1);
        exr.submit(task2);
     
        exr.shutdown();
        // shutdown(); 하고 나서 아래 명령어를 보면 기다리라고 함.
        exr.awaitTermination(100, TimeUnit.SECONDS);

        System.out.println(cnt.getCount());
    }
}
```

`        exr.awaitTermination(100, TimeUnit.SECONDS);`       

첫번째 인자: 숫자, 두번째 인자: 단위

shutdown(); 메소드는 쓰레드 풀에 전달된 작업이 마무리되면 풀을 폐쇄하라고 명령한다. 그런데 쓰레드 풀에 전달된 작업의 최종 결과를 확인할 시간이 필요하다. 따라서 awaitTermination을 이용한다. 이는 쓰레드 풀에 전달된 작업이 끝나길 100초간 기다린다는 의미이다.

아래 둘 중 한가지 상황에 이르러야 메소드를 빠져나오게 된다.

* 쓰레드 풀에 전달된 모든 작업이 완료된다.
* (작업이 완료되지는 않았지만) 100초를 다 기다렸다.

### 컬렉션 인스턴스 동기화

동기화는 특성 상 필연적인 성능 저하를 수반한다. 불필요하게 동기화를 진행하지 않도록 주의한다. 

이런 이유로 컬렉션 프레임워크의 클래스 대부분도 동기화 처리 되어있지 않다. 따라서 쓰레드의 동시 접근에 안전하지 않다. 대신에 Collections의 다음 메소드를 통한 동기화 방법을 제공하고 있다.

```java
List<String> lst = Collections.synchronizedList(new ArrayList<String>());
```



> 참고: Vector 클래스는 이미 기본적으로 동기화가 되어 있다.

```java
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;
import java.util.ListIterator;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.TimeUnit;

class SyncArrayList {
    public static List<Integer> lst = 
              Collections.synchronizedList(new ArrayList<Integer>());

    public static void main(String[] args) throws InterruptedException {
        for(int i = 0; i < 16; i++)
            lst.add(i);
        System.out.println(lst);

        Runnable task = () -> {
            ListIterator<Integer> itr = lst.listIterator();
                
            while(itr.hasNext())
                itr.set(itr.next() + 1);
        };
/*
        Runnable task = () -> {
            synchronized(lst) {
                ListIterator<Integer> itr = lst.listIterator();
                
                while(itr.hasNext())
                    itr.set(itr.next() + 1); 
            }
        };

*/
        ExecutorService exr = Executors.newFixedThreadPool(3);
        exr.submit(task);
        exr.submit(task);
        exr.submit(task);
     
        exr.shutdown();
        exr.awaitTermination(100, TimeUnit.SECONDS);
        System.out.println(lst);
    }
}
```

