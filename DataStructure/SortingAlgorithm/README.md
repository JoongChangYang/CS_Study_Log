# 정렬 알고리즘

### Contents

- [선택 정렬(Selection sort)](#선택-정렬Selection-Sort)
- [구현](#구현)



## 선택 정렬(Selection sort)

- **가장 작은것** 을 선택해서 앞으로 보내는 정렬 기법
- **가장 작은것** 을 선택하는데에 **N** 번 앞으로 보내는데에 **N** 번의 연산으로 **O(N<sup>2</sup>)** 의 시간복잡도를 가진다

<img src="/Assets/SelectionSort.JPG" width="70%">

``` c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <limits.h>
#define SIZE 1000

int arr[SIZE];

int swap(int* a, int* b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}

int main(void) {

	int size, min, index;
	printf("배열 크기 입력\n");
	scanf("%d", &size);

	printf("배열에 들어갈 데이터 %d개 입력\n", size);

	for (int i = 0; i < size; i++) {
		scanf("%d", &arr[i]);
	}

	for (int i = 0; i < size; i++) {
		
		min = INT_MAX;

		for (int j = i; j < size; j++) {
			if (min > arr[j]) {
				min = arr[j];
				index = j;
			}
		}
		swap(&arr[i], &arr[index]);
	}

	for (int i = 0; i < size; i++) {
		printf("%d ", arr[i]);
	}

	printf("\n");

	/*
	실행 결과

	배열 크기 입력
	10
	배열에 들어갈 데이터 10개 입력
	5
	10
	9
	6
	1
	3
	2
	8
	7
	4
	1 2 3 4 5 6 7 8 9 10
	*/

	system("pause");
	return 0;
}
```

