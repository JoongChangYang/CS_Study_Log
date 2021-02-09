# 자료구조 개요

> 자료구조의 필요성의 이해
>
> 자료구조의 성능 측정 방법



## Contents

- [개요](#개요)
- [자료구조의 종류](#자료구조의-종류)
- [성능 측정](#성능-측정)



### 개요

- **자료구조**란 데이터를 효과적으로 저장하고, 처리하는 방법이다



### 자료구조의 종류

#### 선형 구조

- 배열(Array)
- 연결 리스트(Linked Linst)
- 스택(Stack)
- 큐(Queue)



#### 비선형 구조

- 트리(Tree)
- 그래프(Graph)



### 성능 측정

> 효율적인 알고리즘을 사용한다고 가정했을 때 일반적으로 시간과 공간은 반비례 관계이다

#### 시간 복잡도(Time Complexity)

> 알고리즘에 사용되는 연산 횟수

- **시간 복잡도**를 표현할 때는 최악의 경우를 나타내는 **Big-O** 표기법을 사용한다

  - 다음 알고리즘은 **O<sub>(n)</sub>** 의 시간 복잡도를 가진다

    ``` c
    int n = 10;
    
    // n 만큼 반복하기 때문에 O(n)의 시간복잡도를 가짐
    for (int i = 0; i < n; i++) {
    		printf("%d\n", i);
    }
    ```

  - 다음 알고리즘은 **O<sub>(n<sup>2</sup>)</sub>** 의 시간 복잡도를 가진다

    ``` c
    int n = 10;
    
    // 2중 for문을 하여 n*n 만큼 반복했기 때문에 O(n^2)의 시간복잡도를 가짐
    for (int i = 0; i < n; i++) {
    
        for (int j = 0; j < i; j++) {
            printf("%d, %d\n", i, j);
        }
    
    }
    ```

  - **n**이 1,000일때

    > 보통 **10억**을 넘어가면 1초 이상의 시간이 소요됨

    - **n **: 1,000번의 연산
    - **nlogn** : 약 10,000번의 연산
    - **n<sup>2</sup>** : 약 1,000,000번의 연산
    - **n<sup>3</sup>** : 약 1,000,000,000번의 연산
    
    <img src="https://github.com/JoongChangYang/TIL_C/blob/main/Assets/HelloDataStructure_TimeComplexity.PNG" width="50%">HelloDataStructure_TimeComplexity</img>
    ![HelloDataStructure_TimeComplexity](https://github.com/JoongChangYang/TIL_C/blob/main/Assets/HelloDataStructure_TimeComplexity.PNG)

- **시간 복잡도**를 표기할 때는 항상 큰 항과 계수만 표시한다

  **O(3n<sup>2</sup> + n)** = **O(n<sup>2</sup>)**

#### 공간 복잡도(Space Complexity)

> 알고리즘에 사용되는 메모리의 양

- **공간 복잡도**를 표기할 때는 일반적으로 **MB** 단위로 표기한다

``` 
int array[1000]: 4KB
int array[1000000]: 4MB
int array[2000][2000]: 16MB
```

