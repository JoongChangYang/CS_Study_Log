# 전처리기(Preprocesser)

## Contents

- [개념](#개념)
- [전처리기의 종류](#전처리기의-종류)
- [조건부 컴파일](#조건부-컴파일)
- [파일 분할 컴파일](#파일-분할-컴파일)



### 개념

- 전치리기는 프로그램을 컴파일 할 때 컴파일 직전에 실행되는 별도의 프로그램이다
- 전처리기가 실행되면 각 코드 파일에서 **지시자(directives)**를 찾는다 
- **지시자(directives)** 는 `#`으로 시작해서 줄 바꿈으로 끝나는 코드이다
- 전처리기 구문은 다른 프로그램 영역과 독립적으로 처리된다
- 전처리기 구문은 소스코드 파일 단위로 효력이 존재한다



### 전처리기의 종류

#### 파일 포함 전처리기

> `#include`

- `#include`는특정 파일을 라이브러리로서 포함시키기 위해 사용한다 

  - 말 그대로 파일의 소스코드 자체를 다 가져오는것이기 때문에 한번 참조한 헤더 파일을 여러 번 참조하지 않도록 유의해야 한다 

- `#include` 구문으로 가져올 수 있는 파일에는 제약이 없다

- `#include`는 두가지 방법으로 사용할 수 있다

  - `<>`는 컴파일러와 함께 제공되는 헤더 파일을 **include** 할 때 사용한다. 

    - 해당 헤더 파일은 런타임 라이브러리의 헤더 파일로서 운영체제의 특별한 위치에(**시스템 디렉토리**) 존재한다
    - 운영체제마다 시스템 디렉토리가 존재하는 경로가 다를 수 있다

    ``` c
    #include <filename>
    ```

  - `""`는 소스 파일이 있는 디렉터리에서 헤더 파일을 **include**하도록 **전처리기(preprocesser)**에게 지시한다 

    - 만약 현재 소스파일이 있는 디렉터리에 파일이 없다면 시스템 디렉토리에서 파일을 검색한다
    - 일반적으로 이와 같은 방법으로 자신이 작성한 헤더 파일을 **include**한다

    ``` c
    #include "filename"
    ```



#### 매크로 전처리기

> `#define`

- 프로그램 내에서 사용되는 상수나 함수를 매크로 형태로 저장하기 위해 사용

- 기본 형태 : `#define [name] [value]`

  ``` c
  #include <stdio.h>
  #define PI 3.1415926535 // 매크로 상수 값을 선언
  #define ll long long // 매크로 타입 별칭 선언
  #define ld long double // 매크로 타입 별칭 선언
  
  int main(void) {
  	int r = 10;
  	printf("원의 둘레: %.2f\n", 2 * PI * r);
  
  	ll longlongValue = 9854321349502;
  	ld longdoubleValue = 1000.12345;
  
  	printf("long long value: %lld\n", longlongValue);
  	printf("long double value %Lf\n", longdoubleValue);
  
  	/*
  	실행 결과
  	원의 둘레: 62.83
  	long long value: 9854321349502
  	long double value 1000.123450
  	*/
  
  	system("pause");
  	return 0;
  }
  ```

- 인자를 받아 연산처리를 하는 형태 : `#define [name]([parameter] ... ) ([value]) `

  ``` c
  #include <stdio.h>
  #define MULTIFLY(x, y) (x * y)
  
  int main(void) {
  	int x = 3;
  	int y = 15;
  	printf("%d * %d = %d\n", x, y, MULTIFLY(x, y));
  
  	/*
  	실행 결과
  	3 * 15 = 45
  	*/
  
  	system("pause");
  	return 0;
  }
  ```



### 조건부 컴파일

- 조건부 컴파일은 컴파일이 이루어지는 영역을 지정하는 기법이다
- 일반적으로 **디버깅**과 **소스코드 이식**을 목적으로 하여 작성됨
- C언어로 시스템 프로그램을 작성할 때에는 운영체제에 따라서 소스코드가 달라질 수 있다 
- 이때 운영체제에 따라서 컴파일이 수행되는 소스코드를 다르게 할 수 있다

#### #if 

- `#if` 지시자를 이용한 조건부 컴파일은 C언어의 if 조건문과 거의 비슷함
- `#if` 지시자 다음에 나오는 **조건식**의 결과가 **0**이 아니면 **참**, **0**이면 거짓으로 간주함
- 반드시 `#endif` 지시자를 사용하여 **조건부 컴파일의 끝을 명시해야한다**

``` c
#if 조건식1
    // 컴파일할 명령문1
#elif 조건식2
    // 컴파일할 명령문2
#else
    // 컴파일할 명령문3
#endif
```



#### #ifdef 

- `#ifdef`는 **if defined**의 약어로 `#ifdef`지시자 다음에 나오는 **매크로** 이름과 같은 **매크로**가 이미 정의되어있으면 내부 구문을 컴파일 하고 아닐 경우 다음으로 넘어간다
- 흔히 헤더 파일의 내용이 중복되어 사용되지 않도록 하기위해 사용한다

``` c
#ifdef 매크로이름 // 정의된 매크로 이름이 있으면 하단 컴파일
	// 컴파일할 명령문1
#elif 조건식
	// 컴파일할 명령문2
#else
	// 컴파일할 명령문3
#endif  
```



#### #ifndef 

- `#ifndef`는 **if not defined**의 약어로 `#ifdef`지시자와 정 반대의 조건을 검사한다
- `#ifndef`지시자 다음에 오는 **매크로**이름과 같은 **매크로**가 이미 정의되어있지 **않을때** 내부 구문을 컴파일 한다

``` c
#ifndef 매크로이름
	// 컴파일 할 명령문1
#elif
	// 컴파일 할 명령문2
#else
	// 컴파일 할 명령문3
#endif
```

- 예시

  - 기본적으로 `#include`는 소스 코드를 현재 파일에 **포함** 하는 것이기 때문에 중복되면 컴파일 에러가 발생한다

    **PreprocessExample.h**

    ``` c
    int add(int a, int b) {
    	return a + b;
    }
    ```

    **main.c**

    ``` c
    #include <stdio.h>
    #include "PreprocesserExample.h"
    // #include "PreprocesserExample.h" 같은 헤더 파일이 중복 include되면 오류 발생
    
    int main(void) {
    
    	system("pause");
    	return 0;
    }
    ```

  - 실수로 중복 `#include`를 했을때를 대비해 조건부 컴파일 문법을 사용하여 컴파일 에러를 막을 수 있다

    **PreprocessExample.h**

    ``` c
    // _Preprocesser_Example_H_이름으로 매크로가 정의되지 않았으면 내부 구문을 컴파일
    #ifndef _Preprocesser_Example_H_ 
    
    // 다음에 들어왔을 때는 컴파일 되지 않도록 _Preprocesser_Example_H_ 매크로 상수를 정의
    #define _Preprocesser_Example_H_
    int add(int a, int b) {
    	return a + b;
    }
    #endif
    ```

    **main.c**

    ``` c
    #include <stdio.h>
    #include "PreprocesserExample.h"
    #include "PreprocesserExample.h"
    
    // 정상 빌드 성공!!
    int main(void) {
    
    	system("pause");
    	return 0;
    }
    ```



### 파일 분할 컴파일

- 일반적으로 자신이 직접 라이브러리를 만들때에는 **C언어 파일**과, **헤더 파일**을 모두 작성해야 한다



#### PreprocesserExample.h

> 함수들을 **정의** 하는 헤더 파일

``` c
#ifndef _Preprocesser_Example_H_ 

#define _Preprocesser_Example_H_

int add(int a, int b); // 함수의 정의만 한다

#endif
```



#### PreprocesserExample.c

> 헤더파일에 정의된 함수들을 구현하는 파일

``` c
#include "PreprocesserExample.h" // 구현 할 때 해당 헤더 파일을 include 해줘야 한다 

int add(int a, int b) {
	return a + b;
}
```



#### main.c

> 메인 함수가 포함된 메인 파일

``` c
#include <stdio.h>
#include "PreprocesserExample.h"

int main(void) {
	printf("%d\n", add(3, 6));

	/*
	실행 결과
	9
	*/
    
	system("pause");
	return 0;
}
```

