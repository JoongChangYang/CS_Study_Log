# 변수(Variable)와 상수(Constant)

> 기본적인 자료형, 변수와 상수

## Contents

- [변수](https://github.com/JoongChangYang/TIL_C/blob/main/Variable_And_Constant.md#%EB%B3%80%EC%88%98variable)
- [상수](https://github.com/JoongChangYang/TIL_C/blob/main/Variable_And_Constant.md#%EC%83%81%EC%88%98constant)



### 자료형(Data Type)

![Data Type](https://github.com/JoongChangYang/TIL_C/blob/main/Assets/C_DataType.png)



### 예약어와 식별자

- 식별자(Identifier)란 변수나 함수 등의 고유한 이름을 지정할때 사용한다.
- 이때 C언어 문법으로 정해진 예약어는 식별자로 사용할 수 없다. `ex)` `string, for, if...`



### 변수(Variable)

> 변할 수 있는 데이터

- 변수의 선언

  - 변수를 선언할때 초기값을 적용할 수 있다.

    ``` c
    int someNumber;
    int somNumber = 7;
    ```

  - 변수를 사용하려면 초기화 되어있어야 하며 초기화 되지 않은 변수는 쓰레기 값이 들어간다.

    - 초기화되지 않은 변수를 사용하려고 하면 에러를 출력한다.

    ``` c
    int someNumber;
    printf("The number is %d.\n");
    // C6001	초기화되지 않은 메모리 'someNumber'을(를) 사용하고 있습니다.
    ```

  - 정적 변수로 선언된것은 기본적으로 0으로(int Type의 경우) 값이 초기화 된다.

    ``` c
    #include <stdio.h>
    
    static int someNumber;
    
    int main(void) {
    	printf("The number is %d.\n", someNumber);
    	system("pause");
    	return 0;
    }
    // The number is 0.
    ```
  
  

### 상수(Constant)

> 변할 수 없는 데이터

- 상수의 선언

  ``` c
  const int constantNumber = 100;
  ```

  

  