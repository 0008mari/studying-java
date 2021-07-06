# Chapter 2 변수와 자료형

이번 챕터도 날로먹어보겠다

## 02-1 변수의 이해와 활용

자바에서 boolean 형은 1바이트. true, false만 표현 가능 (즉, 정수와 연산 불가)

컴퓨터의 실수 표현에는 오차가 존재한다. 따라서 연산 결과에도 오차가 존재한다.

변수 이름 짓기 : 예약어 불가, 숫자로 시작하면 안됨, 허용되는 특수문자는 $, _

## 02-2 정수의 표현 방식 이해하기

굉장히 본격적인 책이네

### 음의 정수 표현하기

가장 왼쪽 비트 MSB: 이걸로 부호 판단, MSB에 따라 수 해석 방법 다름 

음의 정수 표현: 양의 정수의 이진수 표현 → 2의 보수화

## 02-3 실수의 표현 방식 이해하기

[https://s3.us-west-2.amazonaws.com/secure.notion-static.com/725e69a9-78c5-4716-87e0-60af92239c2d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210706%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210706T111024Z&X-Amz-Expires=86400&X-Amz-Signature=1e57aeaefc10f7b2751e0301c4cec21a168e28ac0c30496498f023fb4dfda059&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22]

부호 + 지수 + 가수(만티싸)

## 02-4 자바의 기본 자료형

### 정수: byte, short, int, long

연산이 필요한 데이터는 int로 하자.

### 실수: float, double

정밀도의 차이가 있으며, 더블이 더 크다

### 문자: char

유니코드: 문자 하나를 2바이트로 표현

문자(character)는 작은따옴표로 묶어서 표현

실제 저장되는 것은 해당 문자의 유니코드 값이다.

### 논리: boolean

true, false

---

끝
