# Chapter 1 렛츠 스타트 자바~

# 자바 가상머신

운영체제가 자바 가상머신을 실행시키고, 자바 가상머신이 자바 프로그램을 실행시킨다.

운영체제에 맞는 가상머신의 설치가 필요하다. 프로그램은 운영체제에 상관 없이 JVM 위에서 실행된다.

# 자바 컴파일러

소스코드를 JVM이 이해할 수 있는 자바 바이트코드로 변환한다.

한편 자바 런처는 jvm을 구동하고 그 위에 자바 프로그램이 실행되도록 돕는다. 

# Hello, World!;

```jsx
class FirstJavaProgram
{
	public static void main(String[] args)
	{
		System.out.printIn("Hello, Java!");
	}
}	
```

System.out.printIn : 따옴표 내부를 출력. 숫자의 경우 따옴표 불필요. + 연산자 이용해 concat

숫자의 경우 (3+5) 하면 8 출력

# 주석

```jsx
/* 블록 단위 주석 */
// line 단위 주석
```

들여쓰기 필요

아싸 끝