# 동적 메모리 할당

> 프로그램 실행 도중에 메모리가 할당 되도록 한다.



## Contents

- [개념](#개념)
- [동적 메모리 사용](#동적-메모리-사용)



### 개념

- 일반적으로 C언어에서 배열의 경우 사전에 적절한 크기만큼 할당해주어야 한다
- 우리가 원하는 만큼만 메모리를 할당해서 사용하고자 한다면 동적 메모리 할당을 사용한다
- **동적**이라는 말의 의미는 **'프로그램 실행 도중에'** 라는 의미이다
- 동적으로 할당된 변수는 **힙 영역**에 저장된다



### 동적 메모리 사용

#### 메모리 할당

> `malloc()`

- C언어에서는 `malloc()` 함수를 이용해 원하는 만큼의 메모리 공간을 확보할 수 있다
- `malloc()` 함수는 메모리 할당에 성공하면 주소를 반환하고, 그렇지 않으면 **NULL**을 반환한다
- `malloc()` 함수는 `<stdlib.h>` 라이브러리에 정의되어있다
- 동적 메모리 할당을 수행할 때마다 할당되는 포인터의 주소는 변칙적이다
- 호출 방법: `malloc(할당할 바이트 크기);`

``` c
#include <stdio.h>
#include <stdlib.h>

int main(void) {

	int *someNumber = malloc(sizeof(int)); // int 타입 크기만큼(4바이트) 메모리 할당
	printf("첫번째 동적할당 주소: %d\n", someNumber);

	someNumber = malloc(sizeof(int)); // int 타입 크기만큼(4바이트) 메모리 재할당
	printf("두번째 동적할당 주소: %d\n", someNumber);

	/*
	실행 결과
	첫번째 동적할당 주소: 23615224
	두번째 동적할당 주소: 23615272
	*/
	
    system("pause");
	return 0;
    
}
```



#### 메모리 해제

> `free()`

- 전통적인 C언어에서는 스택에 선언된 변수는 따로 메모리 해제를 해주지 않아도 된다
- 반면 **동적으로 할당된 변수는 반드시 `free()` 함수로 메모리 해제**를 해주어야 한다
- 메모리 해제를 하지않으면 메모리 내의 프로세스 무게가 더해져 언젠가는 오류가 발생한다
- **메모리 누수(Memory Leak)** 방지는 코어 개발자의 핵심 역량이다

``` c
#include <stdio.h>
#include <stdlib.h>

int main(void) {

	int* numberOfPointer = malloc(sizeof(int));
	printf("첫번째 동적할당 포인터 주소: %d\n", numberOfPointer);

	free(numberOfPointer);
	printf("첫번째 메모리 해제 이후 주소: %d\n", numberOfPointer);

	numberOfPointer = malloc(sizeof(int));
	printf("두번째 동적할당 포인터 주소: %d\n", numberOfPointer);

	free(numberOfPointer);
	printf("두번째 메모리 해제 이후 주소: %d\n", numberOfPointer);

	/*
	첫번째 동적할당 포인터 주소: 24532816
	첫번째 메모리 해제 이후 주소: 24532816
	두번째 동적할당 주소: 24532816
	두번째 메모리 해제 이후 주소: 24532816

	해제를 해도 왜 주소값을 가지고있는지는 아직 모르겠..
	*/

	system("pause");
	return 0;
}
```



#### 동적으로 문자열 처리

> `memset(포인터, 값, 크기);`

- 일괄적인 범위 내의 메모리를 모두 특정한 값으로 설정하기 위해서는 `memset()`을 사용한다
- **1Byte**씩 값을 저장하므로 문자열 배열의 처리 방식과 흡사하다
- 따라서 `memset()` 함수는 `<string.h>`라이브러리에 선언되어 있다

``` c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void) {

	char* someChar = malloc(100); // 100바이트의 메모리 동적 할당
	memset(someChar, 'A', 100); // someChar의 공간을 모두 'A'로 채운다

	for (int i = 0; i < 100; i++) {
		printf("%c ", someChar[i]);
	}

	/*
	실행 결과
	A A A A ....(100개)
	*/

	system("pause");
	return 0;
}
```



#### 종합 예제

> 분석 후 내용 추가 예정

``` c
#include <stdio.h>
#include <stdlib.h>

int main(void) {

	int** ptr = (int**)malloc(sizeof(int*) * 3);

	for (int i = 0; i < 3; i++) {
		*(ptr + i) = (int*)malloc(sizeof(int) * 3);
	}

	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			*(*(ptr + i) + j) = i * 3 + j;
		}
	}

	for (int i = 0; i < 3; i++) {
		for (int j = 0; j < 3; j++) {
			printf("%d ", *(*(ptr + i) + j));
		}
		printf("\n");
	}
    
	/*
	* 실행 결과
	0 1 2
	3 4 5
	6 7 8
	*/
	system("pause");
	return 0;
}
```

