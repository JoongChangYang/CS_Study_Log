# 스택을 활용한 계산기 만들기

> 스택을 활용하여 계산기를 구현



### Contents

- [개요](#개요)
- [구현](#구현)



## 개요

- **중위 표기법** : 일반적으로 사람이 수식을 표기할 때 사용하는 표기 방법
  - **예시** : 7 * 5 + 3
- **후위 표기법** : 컴퓨터가 계산하기에 편한 수식의 형태
  - 후위 표기법에서 연산자는 **뒤쪽에** 위치한다
  - **예시** : 7 5 * 3 +

## 구현

> 분석 후 내용 추가 예정

- 스택을 활용해 수식을 계산하는 방법
  1. 수식을 **후위 표기법** 으로 변환
  2. **후위 표기법** 을 계산하여 결과 도출
- **중위 표기법** 을 **후위 표기법** 으로 바꾸는 방법
  - `피연산자` 가 들어오면 바로 출력 한다
  - `연산자` 가 들어오면 자기보다 우선순위가 높거나 같은것들을 빼 출력하고 자신을 스택에 담는다
  - `(` 를 만나면 무조건 스택에 담는다
  - `)` 를 만나면 `(` 를 만날때까지 스택에서 출력한다

### Stack

> 스택의 정의와 동작을 담당하는 모듈

- **stack.h**

  ``` c
  #ifndef STACK_H
  #define STACK_H
  
  // 스택에서 사용될 노드 정의
  typedef struct Node {
  	char data[100];
  	struct Node* next;
  } Node;
  
  // 스택 정의
  typedef struct Stack {
  	Node* top;
  } Stack;
  
  // 스택의 최상위 데이터를 반환(스택에서 꺼내지는 않음)
  char* getTop(struct Stack* stack);
  
  // 스택에 데이터를 push
  void push(struct Stack* stack, char* data);
  
  // 스택 pop
  char* pop(struct Stack* stack);
  
  #endif
  ```

- **stack.c**

  ``` c
  #include "stack.h"
  #include <stdio.h>
  
  char* getTop(Stack* stack) {
  	Node* top = stack->top;
  	return top->data;
  }
  
  void push(Stack* stack, char* data) {
  	Node* node = (Node*)malloc(sizeof(Node));
  	strcpy(node->data, data);
  	node->next = stack->top;
  	stack->top = node;
  }
  
  char* pop(Stack* stack) {
  	if (stack->top == NULL) {
  		printf("스택 언더플로우가 발생했습니다\n");
  		return NULL;
  	}
  
  	Node* node = stack->top;
  	char* data = (char*)malloc(sizeof(char) * 100);
  	strcpy(data, node->data);
  	stack->top = node->next;
  	free(node);
  	return data;
  }
  ```

### Calulate

> 계산을 담당하는 모듈

- **calculate.h**

  ``` c
  #ifndef CALCULATOR_H
  #define CALCULATOR_H
  
  #include "stack.h"
  
  // 연산자인지 아닌지를 구분하는 함수: 연산자이면 1을 연산자가 아니면 0을 반환
  int isOperator(char* s);
  
  // 들어온 문자열의 우선순위를 반환하는 함수
  int getPriority(char* i);
  
  // 후위 표기법으로 변환하는 함수
  char* transition(Stack* stack, char** s, int size);
  
  // 연산 함수
  void calculate(Stack* stack, char** s, int size);
  
  #endif
  ```

- **calculate.c**

  ``` c
  #define _CRT_SECURE_NO_WARNINGS
  #include "calculate.h"
  #include <stdio.h>
  #include <string.h>
  #include <stdlib.h>
  
  int isOperator(char* s) {
  	return !strcmp(s, "+") || !strcmp(s, "-") || !strcmp(s, "*") || !strcmp(s, "/");
  }
  
  int getPriority(char* i) {
  	if (!strcmp(i, "(")) return 0;
  	if (!strcmp(i, "+") || !strcmp(i, "-")) return 1;
  	if (!strcmp(i, "*") || !strcmp(i, "/")) return 2;
  	return 3;
  }
  
  char* transition(Stack* stack, char** s, int size) {
  	char result[1000] = "";
  
  	for (int i = 0; i < size; i++) {
  		if (isOperator(s[i])) {
  
  			while (stack->top != NULL && getPriority(getTop(stack)) >= getPriority(s[i])) {
  				// 연산자가 나오면 현재 연산자보다 우선순위가 낮은 연산자들을 모두 pop하여 result에 더한다
  				strcat(result, pop(stack));
  				strcat(result, " ");
  			}
  
  			// 현재 연산자를 스택에 push
  			push(stack, s[i]);
  
  		}
  		else if (!strcmp(s[i], "(")) {
  			// "(" 가 나오면 스택에 바로 push
  
  			push(stack, s[i]);
  		}
  		else if (!strcmp(s[i], ")")) {
  
  			while (strcmp(getTop(stack), "(")) {
  				// ")"가 나오면 "("를 만날때까지 스택에서 pop을 하여 result에 더한다
  				strcat(result, pop(stack));
  				strcat(result, " ");
  			}
  
  			// "(" 를 pop 한다 
  			pop(stack);
  		}
  		else {
  			// 피연산자가 나오면 result에 더한다
  			strcat(result, s[i]);
  			strcat(result, " ");
  		}
  	}
  
  	while (stack->top != NULL) {
  		// 스택에 남아있는 모든 값을 result에 더한다
  		strcat(result, pop(stack));
  		strcat(result, " ");
  	}
  
  	// 최종 결과 문자열 반환
  	return result;
  }
  
  void calculate(Stack* stack, char** s, int size) {
  	/*
  	x, y : 피연산자
  	z : 결과값
  	*/
  	int x, y, z;
  
  	for (int i = 0; i < size; i++) {
  
  		if (isOperator(s[i])) {
  			// 연산자가 들어오면 스택에서 숫자 두개를 꺼내 x, y에 할당
  			// atoi : 문자를 int형으로 변환하는 함수
  			x = atoi(pop(stack));
  			y = atoi(pop(stack));
  
  			// 연산 할때 y를 앞에 두는 이유는 스택에 먼저 들어간 수를 y에 할당하기 때문
  			if (!strcmp(s[i], "+")) z = y + x;
  			if (!strcmp(s[i], "-")) z = y - x;
  			if (!strcmp(s[i], "*")) z = y * x;
  			if (!strcmp(s[i], "/")) z = y / x;
  
  
  			char buffer[100]; // 연산 결과를 문자열로 받을 buffer 변수 선언
  
  			sprintf(buffer, "%d", z); // 연산 결과를 문자열로 buffer에 할당
  
  			push(stack, buffer); // 문자열로 받은 연산 결과를 스택에 push
  		}
  		else {
  			push(stack, s[i]); // 연산자 이외의 값(피연산자)이 나오면 스택에 push
  		}
  	}
  
  	// 결과값 출력
  	printf("계산 결과 : %s\n", pop(stack));
  }
  ```

### Main

> 메인에서 위에 정의한 stack, calculate를 `#include`하여 사용

- **main.c**

  ``` c
  #define _CRT_SECURE_NO_WARNINGS
  #include <stdio.h>
  #include <stdlib.h>
  #include <string.h>
  #include "stack.h"
  #include "calculate.h"
  
  int main(void) {
  
  	// 스택 초기화
  	Stack stack;
  	stack.top = NULL;
  
  	// 계산식 할당
  	char solution[100] = "( ( 3 + 4 ) * 5 ) - 5 * 7 * 5 - 5 * 10"; // -190
  
  	// 계산에 필요한 size값 선언
  	int size = 1;
  
  	printf("solution : %s\n", solution);
  
  	// solution의 공백을 기준으로 문자의 개수를 구한다(size)
  	for (int i = 0; i < strlen(solution); i++) {
  		if (solution[i] == ' ') size++;
  	}
  	
  	// solution을 공백을 기준으로 잘라 받을 포인터 선언
  	char* solutionToken = strtok(solution, " ");
  
  	// 계산에 직접적으로 들어갈 문자열 배열 동적 메모리 할당
  	char** input = (char**)malloc(sizeof(char*) * size);
  
  	// input 안에 실질적으로 문자열이 들어갈 수 있도록 메모리 동적 할당
  	for (int i = 0; i < size; i++) {
  		input[i] = (char*)malloc(sizeof(char) * 100);
  	}
  	
  	// solution 문자열을 공백을 기준으로 계속 잘라 input에 원소로 넣어준다
  	for (int i = 0; i < size; i++) {
  		strcpy(input[i], solutionToken);
  		solutionToken = strtok(NULL, " ");
  	}
  
  	// 후위 표기법으로 바뀐 문자열이 들어갈 변수 선언
  	char solutionWithTransition[1000];
  	// 후위 표기법으로 변환하는 함수(transition)를 호출하여 반환 받은 문자열을 solutionWithTransition에 복사
  	strcpy(solutionWithTransition, transition(&stack, input, size));
  	
  	printf("후위 표기법: %s\n", solutionWithTransition);
  
  	// solution의 데이터 갯수를 할당한 size 변수를 solutionWithTransition의 데이터 갯수를 할당하기 위해 1로 초기화
  	size = 1;
  
  	// solutionWithTransition의 데이터 갯수를 세어 size에 할당
  	// 후위 표기법으로 변환할 때 마지막은 항상 공백이 들어가므로 1 빼기(calulate.c -> transition 참조)
  	for (int i = 0; i < strlen(solutionWithTransition) - 1; i++) { 
  		if (solutionWithTransition[i] == ' ') size++;
  	}
  
  	// solutionWithTransition을 공백을 기준으로 잘라 받을 포인터 선언
  	char* solutionWithTransitionToken = strtok(solutionWithTransition, " ");
  
  	// solutionWithTransition 문자열을 공백을 기준으로 계속 잘라 input에 원소로 넣어준다
  	for (int i = 0; i < size; i++) {
  		strcpy(input[i], solutionWithTransitionToken);
  		solutionWithTransitionToken = strtok(NULL, " ");
  	}
  
  	// 후위 표기법으로 변환된 데이터를 기반으로 계산
  	calculate(&stack, input, size);
  
  	/*
  	실행 결과
  	solution : ( ( 3 + 4 ) * 5 ) - 5 * 7 * 5 - 5 * 10
  	후위 표기법: 3 4 + 5 * 5 7 * 5 * - 5 10 * -
  	계산 결과 : -190
  	*/
  
  	system("pause");
  	return 0;
  }
  ```