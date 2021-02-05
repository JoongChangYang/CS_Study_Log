# 함수 포인터

> 함수의 개념과 활용



## Contents

- [개념](#개념)
- [기본 함수 포인터 활용](#기본-함수-포인터-활용)



### 개념

- C언어 프로그램의 모든 함수는 내부적으로 포인터 형태로 관리할 수 있습니다

- C언어에서는 함수의 이름을 이용해 특정 함수를 호출한다
- 함수 이름은 메모리 주소를 반환한다

``` c
#include <stdio.h>

void someFunction() {
	printf("It is function!\n");
}

int main(void) {

	printf("%d\n", someFunction);

	/*
	실행 결과
	3019317
	*/
    
	system("pause");
	return 0;
}
```

### 함수 포인터 활용

#### 기본 함수 포인터

- 함수 포인터는 특정한 함수의 반환 자료형을 지정하는 방식으로 선언할 수 있다

- 함수 포인터를 이용하면 형태가 같은 서로 다른 기능의 함수를 선택적으로 사용할 수 있다

- 함수 포인터 선언 : `반환 자료형` `(*이름)` `(매개변수...)` = `함수명`;

  - 매개변수 및 반환 자료형이 없는 함수 포인터

    ``` c
    #include <stdio.h>
    
    void myFunction() {
    	printf("It's my function!\n");
    }
    
    void yourFunction() {
    	printf("It's your function!\n");
    }
    
    int main(void) {
    
    	void (*fp)() = myFunction; // 함수 포인터에 myFunction 주소 할당
    	fp(); // 함수 포인터가 가리키는 함수 호출
    	
    	fp = yourFunction; // 함수 포인터에 yourFunction 주소 할당
    	fp(); // 함수 포인터가 가리키는 함수 호출
    
    	/*
    	실행 결과
    	It's my function!
    	It's your function!
    	*/
    	system("pause");
    	return 0;
    }
    ```

  - 매개변수 및 반환 자료형이 없는 함수 포인터

    ``` c
    #include <stdio.h>
    
    int add(int a, int b) {
    	return a + b;
    }
    
    int sub(int a, int b) {
    	return a - b;
    }
    
    int main(void) {
    
    	int (*fp) (int, int) = add; // 함수 포인터에 add 주소 할당
    	printf("%d\n", fp(10, 3)); // 함수 포인터가 가리키는 add 함수 호출
    
    	fp = sub; // 함수 포인터에 sub 주소 할당
    	printf("%d\n", fp(10, 3)); // 함수 포인터가 가리키는 sub 함수 호출
    	
    	/*
    	실행 결과
    	13
    	7
    	*/
    	system("pause");
    	return 0;
    }
    ```


#### 함수 포인터를 반환하는 함수

- 함수 포인터를 반환값으로 사용할 때는 지금까지와는 문법이 조금 다르다
- 선언 : `함수 포인터 반환값 자료형` `(*함수 이름 (매개 변수...))` `(함수 포인터 매개변수...)`

``` c
int (*process(char *message))(int, int) {
	printf("%s\n", message);
	return add;
}

int main(void) {

	printf("결과 값: %d\n", process("10과 20을 더해보겠습니다.")(10, 20));

	/*
	실행 결과
	10과 20을 더해보겠습니다.
	결과 값: 30
	*/

	system("pause");
	return 0;
}
```

