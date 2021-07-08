# Chapter 5 실행 흐름의 컨트롤

## 5-1 if문과 else문

### if, else

```java
if (true or false) {
    true 시 실행되는 영역 
}
```

중첩 가능 

### 조건 연산자 (삼항 연산자)

```java
( condition ) ? (result 1 ) : (result 2)
```



## 5-2 switch와 break문

### switch

```java
switch (n) {
case 1:
case 2:
    ...
default:
}
```

case와 default는 레이블이다. 레이블은 들여쓰기 하지 않는다.

스위치 문에 break 필수

## 5-3 for, while, do~while

## 5-4 break와 continue

블록 탈출

## 5-5 반복문의 중첩

중첩 시에는 for문 우선 고려 (while 보다 중첩 시 간단)

### 레이블을 이용해 break 하기

```java
class LabeledBreakPoint {
    public static void main(String[] args){
outer: for(int i = 1; i < 10; i++) {
    		for (int j = 1; j < 10; j++) {
                if ((i * j) == 72) {
                    System.out.println( ... );
                    break outer;
                }
            }
		}
    }
}
```

break outer 부분 주목. 

outer 레이블을 가장 바깥쪽 for문에 붙였고, 그걸 명시하여 탈출할 수 있다.

레이블은 위치의 지정이 목표이므로, 들여쓰기에 상관 없이 잘 보이는 위치에 놓는다. 