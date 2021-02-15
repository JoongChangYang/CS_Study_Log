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
- 스택을 활용해 수식을 계산하는 방법
  1. 수식을 **후위 표기법** 으로 변환
  2. **후위 표기법** 을 계산하여 결과 도출
- **중위 표기법** 을 **후위 표기법** 으로 바꾸는 방법
  - **피연산자** 가 들어오면 바로 출력 한다
  - **연산자** 가 들어오면 자기보다 우선순위가 높거나 같은것들을 빼고 자신을 스택에 담는다
  - **여는 괄호 '('** 를 만나면 무조건 스택에 담는다
  - **닫는 괄호 ')' **를 만나면 **'('** 를 만날때까지 스택에서 출력한다

## 구현

> 미완성본 내용 추가 예정

``` c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Node {
	char data[100];
	struct Node* next;
} Node;

typedef struct Stack {
	Node* top;
} Stack;

void push(Stack* stack, char* data) {
	Node* node = (Node*)malloc(sizeof(Node));
	strcpy(node->data, data);
	node->next = stack->top;
	stack->top = node;
	printf("push: %s\n", data);
}

char* getTop(Stack* stack) {
	Node* top = stack->top;
	return top->data;
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
	printf("pop: %s\n", data);
	return data;
}

// 들어온 문자열의 우선순위를 반환하는 함수
int getPriority(char* i) {
	if (!strcmp(i, "(")) return 0;
	if (!strcmp(i, "+") || !strcmp(i, "-")) return 1;
	if (!strcmp(i, "*") || !strcmp(i, "/")) return 2;
	return 3;
}

int isOperator(char** s, int i) {
	return !strcmp(s[i], "+") || !strcmp(s[i], "-") || !strcmp(s[i], "*") || !strcmp(s[i], "/");
}

// 후위 표기법으로 변환
char* transition(Stack* stack, char** s, int size) {
	char result[1000] = "";

	for (int i = 0; i < size; i++) {
		
		if (isOperator) {

			while (stack->top != NULL && getPriority(getTop(stack)) >= getPriority(s[i])) {
				strcat(result, pop(stack));
				strcat(result, " ");
			}
			push(stack, s[i]);

		}
		else if (!strcmp(s[i], "(")) {
			push(stack, s[i]);
		}
		else if (!strcmp, ")") {

			while (!strcmp(getTop(stack), "(")) {
				strcat(result, pop(stack));
				strcat(result, " ");
			}
			pop(stack);
		}
		else {
			strcat(result, s[i]);
			strcat(result, " ");
		}
	}

	while (stack->top != NULL) {
		strcat(result, pop(stack));
		strcat(result, " ");
	}

	return result;
}

void calculate(Stack* stack, char** s, int size) {
	/*
	x, y : 피연산자
	z : 결과값
	*/
	int x, y, z;

	for (int i = 0; i < size; i++) {

		if (isOperator) {
			// atoi : 문자를 int형으로 변환하는 함수
			x = atoi(pop(stack));
			y = atoi(pop(stack));

			// 연산 할때 y를 앞에 두는 이유는 스택에 먼저 들어간 수를 y에 할당하기 때문
			if (!strcmp(s[i], "+")) z = y + x;
			if (!strcmp(s[i], "-")) z = y - x;
			if (!strcmp(s[i], "*")) z = y * x;
			if (!strcmp(s[i], "/")) z = y / x;

			char buffer[100];
			sprintf(buffer, "&d", z);
			push(stack, buffer);
		}
		else {
			push(stack, s[i]);
		}
	}
	printf("%s\n", pop(stack));
}

int main(void) {

	Stack stack;
	stack.top = NULL;
	char solution[100] = "( ( 3 + 4 ) * 5 ) - 5 * 7 * 5 - 5 * 10";
	int size = 1;
	for (int i = 0; i < strlen(solution); i++) {
		if (solution[i] == ' ') size++;
	}

	char* ptr = strtok(solution, " ");
	char** input = (char**)malloc(sizeof(char*) * size);

	for (int i = 0; i < size; i++) {
		input[i] = (char*)malloc(sizeof(char) * 100);
	}

	for (int i = 0; i < size; i++) {
		strcpy(input[i], ptr);
		ptr = strtok(NULL, " ");
	}

	char solutionWithTransition[1000];
	strcpy(solution, transition(&stack, input, size));
	printf("후위 표기법: %s\n", solutionWithTransition);

	size = 1;

	// ======================추가 예정==========================
	for (int i = 0; i < strlen; i++) {

	}

	system("pause");
	return 0;
}
```

