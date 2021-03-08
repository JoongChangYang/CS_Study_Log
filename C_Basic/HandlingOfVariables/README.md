# 컴퓨터가 변수를 처리하는 방법

> C언어에서 다양한 변수를 처리하는 방법
>
> 지역변수, 전역변수, 레지스터 변수 등을 학습



## Contents

- [프로그램 메모리 주소](#프로그램-메모리-주소)
- [전역 변수](#전역-변수)
- [지역 변수](#지역-변수)
- [정적 변수](#정적-변수)
- [레지스터 변수](#레지스터-변수)
- [매개 변수](#매개-변수)

### 프로그램 메모리 주소

- 컴퓨터에서 프로그램이 실행되기 위해서는 프로그램이 메모리에 **적재(Load)** 되어야 한다
- 당연히 프로그램의 크기를 충당할 수 있을만큼의 메모리 공간이 있어야 한다
- 일반적인 컴퓨터의 운영체제는 메모리 공간을 네 가지로 구분하여 관리한다

| 코드 영역 |      데이터 영역       |    힙 영역     |        스택 영역        |
| :-------: | :--------------------: | :------------: | :---------------------: |
| 소스코드  | 전역변수<br/>정적 변수 | 동적 할당 변수 | 지역 변수<br/>매개 변수 |



### 전역 변수

- **전역 변수(Global Variable)**란 프로그램의 어디서든 접근 가능한 변수를 말한다
- `main`함수가 실행되기도 전에 프로그램의 시작과 동시에 메모리에 할당된다
- 프로그램의 크기가 커질 수록 전역 변수로인해 프로그램이 복잡해질 수 있다
- 메모리의 **데이터(Data)**영역에 적재된다

``` c
#include <stdio.h>

int globalVariable = 5; // 전역 변수 선언

void changeGlobalVariable() {
	// globalVariable 변수는 전역변수이기 때문에 main함수 바깥에서도 접근이 가능하다
	globalVariable = 10;
}

int main(void) {

	printf("%d\n", globalVariable); // 5
	changeGlobalVariable();
	printf("%d\n", globalVariable); // 10

	system("pause");
	return 0;
}
```



### 지역 변수

- **지역 변수(Local Variable)**란 프로그램에서 특정한 **블록(Block)**에서만 접근할 수 있는 변수를 말한다
- 함수가 실행될 때마다 메모리에 할당되어 함수가 종료되면 메모리에서 해제
- 메모리의 **스택(Stack)**영역에 기록된다

``` c
int main(void) {
	int localVariable = 7; // main 함수의 지역 변수

	if (1) {
		int localVariable = 5; // 조건문의 지역 변수
	}

	printf("%d\n", localVariable); // 7
	// printf()는 main함수 스코프에 있기때문에 7을 출력함

	system("pause");
	return 0;
}
```



### 정적 변수

- **정적 변수(Static Variable)**란 특정 블록에서만 접근할 수 있는 변수이다
- 프로그램이 실행될때 메모리에 할당되어 프로그램이 종료되면 메모리에서 해제된다
- 메모리의 **데이터(Data)** 영역에 적재된다

``` c
#include <stdio.h>

void process() {
	static int staticVariable = 5; // 정적 변수 선언
	staticVariable++;
	printf("%d\n", staticVariable);
}

int main(void) {

	process(); // 6
	process(); // 7
	process(); // 8
	process(); // 9

	/*
	process함수 내부에 정적 변수 staticVariable은 프로그램이 실행됨과 동시에 메모리에 할당됨
	프로그램이 종료되기 전까지 메모리의 Data영역에 남아있기 때문에 값을 기억하고 있다
	*/
	system("pause");
	return 0;
}
```



### 레지스터 변수

- **레지스터 변수(Register Variable)**란 **메인 메모리** 대신 **CPU의 레지스터**를 사용하는 변수이다
- 레지스터는 메인 메모리보다 CPU와 가깝기 때문에 처리속도가 빠르다
- 레지스터는 매우 한정되어 있으므로 실제로 레지스터에서 처리될 지는 장담할 수 없다(컴파일러가 담당함)

``` c
register int registerVariable = 10, i; // 레지스터 변수 선언
for (i = 0; i < registerVariable; i++) {
    printf("%d ", i);
}
/*
실행 결과: 0 1 2 3 4 5 6 7 8 9
*/
```



### 매개 변수

> 함수의 매개변수가 처리될 때

- 함수를 호출할 때 함수에 필요한 데이터를 매개변수로 전달함

- 전달 방식

  - 값에 의한 전달 방식

    단지 값을 전달하므로 함수 내에서 변수가 새롭게 생성됨

    ``` c
    #include <stdio.h>
    
    void add(int a, int b) {
    	a = a + b;
    	printf("add 함수 내부 a 값: %d\n", a);
    	printf("add 함수 내부 a 주소: %d\n", &a);
    }
    
    int main(void) {
    
    	int a = 7;
    
    	add(a, 10);
        printf("\n")
    	printf("add 함수 외부 a 값: %d\n", a);
    	printf("add 함수 외부 a 주소: %d\n", &a);
    	/*
    	실행 결과
    	add 함수 내부 a 값: 17
    	add 함수 내부 a 주소: 7339224
    	
    	add 함수 외부 a 값: 7
    	add 함수 외부 a 주소: 7339440
    
    	함수 외부에서 매개 변수로 내부로 a 변수를 전달했지만 값을 전달한 것뿐이기 때문에 
    	새롭게 메모리 내에 할당되어 처리됨 따라서 외부와 내부의 a 변수는 결국 다른 변수임
    	*/
    
    	system("pause");
    	return 0;
    }
    ```

  - 참조에 의한 전달

    주소를 전달하므로 원래 변수 자체에 접근할 수있음

    ``` c
    #include <stdio.h>
    
    void add(int *a, int b) {
    	*a = *a + b;
    	printf("add 함수 내부 *a 값: %d\n", *a); // 매개변수의 포인터가 가리키는 변수의 값
    	printf("add 함수 내부 *a 주소: %d\n", &*a); // 매개변수의 포인터가 가리키는 변수의 주소값
    	printf("\n");
    	printf("add 함수 내부 a 값: %d\n", a); // 매개 변수의 값(포인터가 가리키는 주소 값)
    	printf("add 함수 내부 a 주소: %d\n", &a); // 매개 변수의 주소값(포인터 변수의 주소값)
    }
    
    int main(void) {
    
    	int a = 7;
    
    	add(&a, 10);
    	
    	printf("\n==============================================\n\n");
    	
    	printf("add 함수 외부 a 값: %d\n", a);
    	printf("add 함수 외부 a 주소: %d\n", &a);
    	/*
    	실행 결과
    	add 함수 내부 *a 값: 17
    	add 함수 내부 *a 주소: 16120800
    
    	add 함수 내부 a 값: 16120800
    	add 함수 내부 a 주소: 16120584
    
    	==============================================
    
    	add 함수 외부 a 값: 17
    	add 함수 외부 a 주소: 16120800
    	
    	add 함수 외부로부터 주소값을 전달하고 내부에서는 포인터 변수로 주소값을 전달 받아서
    	포인터가 가리키는 변수를 찾아서 + 연산을 실행하기 때문에 외부 변수의 값에 변화를 줄 수 있다.
    	*/
    
    	system("pause");
    	return 0;
    }
    ```

    

