# 큐(Queue)

> 큐 자료구조의 개념과 활용 방법



### Contents

- [개요](#개요)
- [용어](#용어)
- [구현](#구현)



## 개요

- 한쪽 끝에서만 자료를 넣고 다른 한쪽에서만 자료를 뺄 수 있는 자료구조
  - **FIFO(First In First Out)** 먼저 들어간 자료가 먼저 나옴
- 스케줄링, 탐색 알고리즘 등 다방면으로 활용된다



## 용어

- 값을 추가하는 것: `Enqueue` 또는 `Put` 또는 `Push`
- 값을 꺼내오는 것: `Dequeue` 또는 `Get` 또는 `Pop`
- **Front** : Dequeue에 사용될 인덱스
- **Rear(Back)** : Enqueue에 사용될 인덱스



## 구현

### 배열을 이용한 큐 구현

- `enqueue` 
  1. `rear` 인덱스에 데이터를 넣는다 
  2. `rear` 인덱스를 1 증가시켜 다음 데이터가 들어갈 인덱스를 가리킨다
- `dequeue`  
  1. `front` 인덱스에 데이터를 넣는다 
  2. `front` 인덱스를 1 증가시켜 다음 데이터를 꺼낼 인덱스를 가리킨다

``` c
#include <stdio.h>
#define SIZE 10000
#define INF 99999999

int queue[SIZE]; // 큐 선언
int front = 0; // 큐의 앞쪽(데이터가 빠져나올) 인덱스
int rear = 0; // 큐의 뒤쪽(데이터가 들어갈) 인덱스

// 데이터 넣기
void enqueue(int data) {

	// rear가 큐의 크기보다 크다면 오버플로우
	if (rear >= SIZE) {
		printf("큐 오버플로우\n");
		return;
	}

	// 큐의 현재 rear인덱스에 데이터를 넣고 rear 1증가
	queue[rear++] = data;
}

// 데이터 빼기
int dequeue() {
	// front가 rear와 같다면 더이상 나올 데이터가 없으므로 언더플로우
	if (front == rear) {
		printf("큐 언더플로우\n");
		return -INF;
	}

	// 큐의 현재 front인덱스의 데이터를 꺼내 반환하고 front 1증가
	return queue[front++];
}

// 출력
void show() {
	printf(" --- 큐의 앞 ---\n");

	for (int i = front; i < rear; i++) {
		printf("%d\n", queue[i]);
	}
	printf(" --- 큐의 뒤 ---\n");
}

int main(void) {

	enqueue(1); // [1]
	enqueue(2); // [1, 2]
	enqueue(3); // [1, 2, 3]
	enqueue(4); // [1, 2, 3, 4]
	dequeue();	// [2, 3, 4]
	dequeue();	// [3, 4]
	enqueue(5);	// [3, 4, 5]
	enqueue(6);	// [3, 4, 5, 6]
	dequeue();	// [4, 5, 6]
	show();

	/*
	실행 결과
	 --- 큐의 앞 ---
	4
	5
	6
	 --- 큐의 뒤 ---
	*/
	system("pause");
	return 0;
}
```



## 연결 리스트를 이용한 큐 구현

- `enqueue` 
  1. **큐** 가 **비어있다면** `front` 에 `node` 할당
  2. **큐** 가 **비어있지 않다면** `rear -> next` 에 `node` 할당
  3. `rear` 에 `node` 를 할당
- `dequeue`
  1. **큐** 가 비어있는지 확인하여 **언더플로우** 를 제어한다
  2. `front` 를 꺼내 **지역 변수** `node`에 할당
  3. `front` 에 `node -> next` 할당
  4. 꺼낸 `node` 에서 데이터를 꺼내 반환하고 **메모리 해제**

``` c
#include <stdio.h>
#include <stdlib.h>
#define INF 99999999

// 큐에서 사용할 Node 정의
typedef struct {
	int data;
	struct Node* next;
} Node;

// Queue 정의
// Queue는 front와 rear만 알고있고 중간에 있는 데이터는 알지못한다
typedef struct {
	Node* front; // 큐의 앞쪽(꺼낼 데이터)
	Node* rear; // 큐의 뒤쪽(들어갈 데이터)
	int count; // 데이터의 갯수
} Queue;

// 데이터 넣기
void enqueue(Queue *queue, int data) {

	Node* node = (Node*)malloc(sizeof(Node)); // 큐에 들어갈 노드의 메모리 동적할당
	node->data = data; // 노드에 데이터 할당
	node->next = NULL; // 노드의 next NULL로 초기화

	
	if (queue->count == 0) {
		// count가 0이라면 큐가 비어있는것 이므로 front에 큐에 들어갈 노드를 할당
		queue->front = node;
	}
	else {
		// count가 0이 아니라면 현재 rear의 next에 큐에 들어갈 노드 할당
		queue->rear->next = node;
	}

	// rear에 큐에 들어갈 노드 할당 후 count 1증가
	queue->rear = node;
	queue->count++;

}

int dequeue(Queue* queue) {
	
	// count가 0이라면 큐가 비어있는것으로 dequeue할 수 없다
	if (queue->count == 0) {
		printf("큐 언더플로우");
		return -INF;
	}

	Node* node = queue->front; // 큐의 front를 꺼내기 위해 node변수에 할당
    
	int data = node->data; // 반환해줄 데이터 추출
    
	queue->front = node->next; // 큐의 front에 꺼낼 노드의 next 할당
    
	free(node); // 꺼낼 노드 메모리 해제
    
	queue->count--; // count 1 감소

	return data;
}

void show(Queue* queue) {
	Node* current = queue->front;
	
	printf(" --- 큐의 앞 ---\n");

	while (current != NULL) {
		printf("%d\n", current->data);
		current = current->next;
	}

	printf(" --- 큐의 뒤 ---\n");
}

int main(void) {

	Queue queue;
	queue.front = queue.rear = NULL;
	queue.count = 0;

	enqueue(&queue, 1); // [1]
	enqueue(&queue, 2);	// [1, 2]
	enqueue(&queue, 3);	// [1, 2, 3]
	enqueue(&queue, 4);	// [1, 2, 3, 4]
	dequeue(&queue);	// [2, 3, 4]
	dequeue(&queue);	// [3, 4]
	enqueue(&queue, 5);	// [3, 4, 5]
	enqueue(&queue, 6);	// [3, 4, 5, 6]
	dequeue(&queue);	// [4, 5, 6]
	dequeue(&queue);	// [5, 6]
	show(&queue);

	/*
	실행 결과

	 --- 큐의 앞 ---
	5
	6
	 --- 큐의 뒤 ---
	*/

	system("pause");
	return 0;
}
```

