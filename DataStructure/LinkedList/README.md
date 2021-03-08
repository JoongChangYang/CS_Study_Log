# 연결 리스트 (Linked List)

> 연결 리스트의 개념과 구현



## Contents

- [개요](#개요)
- [단일 연결 리스트(Single Linked List)](#단일-연결-리스트single-linked-list)
- [양방향 연결 리스트(Double Linked List)](#양방향-연결-리스트double-linked-list)

### 개요

- 삽입과 삭제가 많은 경우 효율적임
- 인덱스 접근이 많은 경우 배열을 이요하는것이 효율적임

- 장점
  - 삽입과 삭제가 배열에 비해서 간단하다는 장점이 있다
  - 필요할 때마다 메모리 공간을 할당 받기 때문에 데이터가 들어갈 공간을 메모리에 미리 할당 할 필요가 없다
- 단점
  - 배열과 다르게 특정 인덱스로 즉시 접근하지 못하며, 원소를 차례대로 검색해야 한다
  - 추가적인 포인터 변수가 사용되므로 메모리 공간이 낭비된다
- 구현
  - 일반적으로 연결 리스트는 구조체와 포인터를 함께 사용하여 구현
  - 연결 리스트는 리스트의 중간 지점에 노드를 추가하거나 삭제할 수 있어야함

#### 배열 기반 리스트

> 배열 기반 리스트의 장단점

- 배열로 만들었으므로 특정 위치의 원소에 즉시 접근할 수 있다는 장점이 있음
- 데이터가 들어갈 공간을 미리 메모리에 할당해야 한다는 단점이 있음 (메모리 공간이 불필요하게 낭비 될 수 있다)
- 원하는 위치로의 삽입이나 삭제가 비 효율적임

``` c
#include <stdio.h>
#define INF 10000

int arr[INF];
int count = 0;

void addBack(int data) {
	arr[count] = data;
	count++;
}

void addFirst(int data) {
    // 데이터를 한 칸씩 뒤로 밀어줘야함
	for (int i = count; i >= 1; i--) {
		arr[i] = arr[i - 1];
	}
	arr[0] = data;
	count++;
}

void removeAt(int index) {
    // addFirst와 마찬가지로 데이터를 한칸씩 당겨줘야함
	for (int i = index; i < count - 1; i++) {
		arr[i] = arr[i + 1];
	}
	count--;
}

void show() {
	for (int i = 0; i < count; i++) {
		printf("%d ", arr[i]);
	}
	printf("\n");
}

int main(void) {

	addFirst(4);
	addFirst(2);
	addFirst(1);
	addBack(7);
	addBack(6);
	addBack(8);
	removeAt(3);
	show();
	/*
	실행 결과
	1 2 4 6 8
	*/
	system("pause");
	return 0;
}
```



### 단일 연결 리스트(Single Linked List)

- 포인터를 이용해 단방향적으로 다음 노드를 가리킨다
- 연결 리스트의 시작 노드를 **헤드(Head)** 라고 하며 별도로 관리 한다
- 다음 노드가 없는 **끝 노드** 의 다음 위치 값으로는 **NULL**을 넣는다

<img src="https://github.com/JoongChangYang/TIL_C/blob/main/Assets/Single_Linked_List.PNG" width="70%">

#### 기본 구현

``` c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
	int data; // 실제 사용될 데이터
	struct Node* next; // 다음 노드를 가리킬 포인터 변수
} Node;

Node *head; // 연결리스트의 시작점인 헤드 노드

int main(void) {

	head = (Node*)malloc(sizeof(Node)); // 헤드에 메모리 동적 할당

	Node* node0 = (Node*)malloc(sizeof(Node));
	node0->data = 1;

	Node* node1 = (Node*)malloc(sizeof(Node));
	node1->data = 2;

	head->next = node0;
	node0->next = node1;
	node1->next = NULL;

	Node* current = head->next; // 현재 노드를 head의 next로 두고 시작

	while (current != NULL) {
		// 노드의 next가 NULL이 나올때까지 next를 찾아 노드의 데이터를 출력
		printf("%d ", current->data);
		current = current->next;
	}

	/*
	실행 결과
	1 2
	*/
	system("pause");
	return 0;
}
```

#### 노드 삽입

<img src="https://github.com/JoongChangYang/TIL_C/blob/main/Assets/Single_Linked_List_InsertNode.PNG" width="70%">

``` c
void insertNode(Node* root, int data) {
	Node* node = (Node*)malloc(sizeof(Node)); // 삽입할 노드 메모리 동적 할당
	node->data = data; // 삽입할 노드에 데이터 할당
	node->next = root->next; // 새로운 노드의 next에 기존 root노드의 next 할당
	root->next = node; // root 노드의 next에 삽입할 노드 할당
}
```



#### 노드 삭제

- **삭제할 노드** 의 이전 노드가 **삭제할 노드** 의 다음 노드를 가리키게 한다

``` c
void removeNode(Node* preNode) {
	Node* removeNode = preNode->next; // 삭제할 노드를 이전 노드에서 할당 받는다
	preNode->next = removeNode->next; // 이전 노드의 next에 삭제할 노드의 next를 할당
	free(removeNode); // 삭제할 노드의 메모리 해제
}
```



### 양방향 연결 리스트(Double Linked List)

- 양방향 연결 리스트는 **머리(Head)** 와 **꼬리(Tail)** 를 모두 가진다
- 양방향 연결 리스트의 각 노드는 **앞 노드** 와 **뒤 노드** 의 정보를 모두 저장한다

<img src="https://github.com/JoongChangYang/TIL_C/blob/main/Assets/Double_Linked_List.PNG" width="70%">

#### 기본 구현

``` c
typedef struct {
	int data; // 실제 사용될 데이터
	struct Node* prev; // 이전 노드를 가리킬 포인터 변수
	struct Node* next; // 다음 노드를 가리킬 포인터 변수
} Node;

Node *head, *tail; // 연결리스트의 시작점과 끝점 노드
```

#### 노드 삽입

<img src="https://github.com/JoongChangYang/TIL_C/blob/main/Assets/Double_Linked_List_InsertNode.PNG" width="70%">

``` c
// 오름차순 삽입 함수 예제코드
void insertNode(int data) {
	Node* node = (Node*)malloc(sizeof(Node)); // 삽입할 노드 메모리 동적 할당
	node->data = data; // 삽입할 노드의 data 값 할당
	
	Node* current = head->next; // head의 next를 current로 지정
	while (current->data < data && current != tail) {
		// 현재 들어온 data보다 큰 값을 가진 노드가 나올때까지 current를 바꿔줌
		current = current->next;
	}

	Node* prev = current->prev; // current를 삽입할 노드 이전 노드로 지정
	prev->next = node; // prev의 next를 삽입할 노드로 할당
	node->prev = prev; // 삽입할 노드의 prev를 prev로 할당
	current->prev = node; // current의 prev를 삽입할 노드로 할당
	node->next = current; // 삽입할 노드의 next를 current로 할당
}
```



#### 노드 삭제

<img src="https://github.com/JoongChangYang/TIL_C/blob/main/Assets/Double_Linked_List_RemoveNode.PNG" width="70%">

``` c
void removeNode(Node* removeNode) {
	Node* prev = removeNode->prev; // 삭제할 노드의 prev 할당
	Node* next = removeNode->next; // 삭제할 노드의 next 할당

	prev->next = next; // prev의 next에 next 할당
	next->prev = prev; // next의 prev에 prev 할당

	free(removeNode); // 삭제할 노드 메모리 헤제
}
```

