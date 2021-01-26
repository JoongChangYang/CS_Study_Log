# 기본 입출력

> C 언어에서 사용되는 기본 입출력



## Contents

- [입력](https://github.com/JoongChangYang/TIL_C/blob/main/Basic_IO.md#%EC%9E%85%EB%A0%A5input)
- [출력](https://github.com/JoongChangYang/TIL_C/blob/main/Basic_IO.md#%EC%B6%9C%EB%A0%A5)
- [형식 지정자](https://github.com/JoongChangYang/TIL_C/blob/main/Basic_IO.md#%ED%98%95%EC%8B%9D-%EC%A7%80%EC%A0%95%EC%9E%90)



### 입력(Input)

- `scanf()` : 특정 변수에 값을 넣기위해 사용

  > 취약 함수로 분류되어있어 `#define _CRT_SECURE_NO_WARNINGS` 명시해야 한다.
  >
  > 입력받을 주소를 나타내기 위해 &를 사용한다.

  - 명시하지 않은 경우

    ``` c
    #include <stdio.h>
    
    int main(void) {
      int inputNumber;
      scanf("%d", &inputNumber);
      printf("입력한 숫자는 %d입니다.\n", inputNumber);
      system("pause");
      return 0;
    }
    
    // 'scanf': This function or variable may be unsafe. Consider using scanf_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.
    ```

  - 명시 한 경우

    ``` c
    #define _CRT_SECURE_NO_WARNINGS
    #include <stdio.h>
    
    int main(void) {
    	int inputNumber; // int 타입 변수 선언
    	scanf("%d", &inputNumber); // 사용자에게 입력받은 데이터를 변수 inputNumber에 할당
    	printf("입력한 숫자는 %d입니다.\n", inputNumber); // 출력
    	system("pause");
    	return 0;
    }
    // 12
    // 입력한 숫자는 12입니다.
    ```



### 출력

- `printf()` : 콘솔에 데이터를 출력

  > 기본적으로 문자열을 출력하며 문자열 내부에 형식 지정자를 사용하여 매개변수로 넘긴 값을 함께 출력할 수 있다.

  `print("문자열 %[형식 지정자]", 매개 변수...)`

  ``` c
  double someDoubleNumber = 1.23;
  printf("%f\n", someDoubleNumber);
  // 1.230000
  
  // 여러개의 값을 출력하는것도 가능
  double someDoubleNumber = 1.23;
  int someIntNumber = 23;
  printf("실수: %f \n정수:%d \n", someDoubleNumber, someIntNumber);
  // 실수: 1.230000
  // 정수:23
  ```



### 형식 지정자

> 데이터 입출력간 데이터 값이 어떤 자료형인지를 지정.
>
> 알맞지 않은 형태의 형식 지정자를 사용하면 오류가 발생함.

#### 구조

- `% [매개 변수] [플래그] [너비] [.정밀도] [길이] 형식 지정자 유형`

#### 형식 지정자 종류

- 정수형

  | 형식 지정자 |          의미          |          예시           | 실행 결과 |
  | :---------: | :--------------------: | :---------------------: | :-------: |
  |   %d, %i    |       10진 정수        | printf(“%d %i”,-20,12); |  -20 12   |
  |     %u      |     양의 10진 정수     |    printf(“%u”,12);     |    12     |
  |     %o      |     양의 8진 정수      |    printf(“%o”,19);     |    23     |
  |     %x      | 양의 16진 정수(소문자) |    printf(“%x”,14);     |     e     |
  |     %X      | 양의 16진 정수(대문자) |    printf(“%X”,14);     |     E     |
  |     %lu     |     unsigned long      |   printf(“%lu”,100);    |    100    |
  |     %ld     |      signed long       |   printf(“%ld”,1000);   |   1000    |
  |    %llu     |   unsigned long long   |   printf(“%llu”,144);   |    144    |
  |    %lld     |    signed long long    |   printf(“%lld”,169);   |    169    |

- 실수형

  | 형식 지정자 |                  의미                  |        예시        |      실행 결과       |
  | :---------: | :------------------------------------: | :----------------: | :------------------: |
  |     %f      |    실수의 소수점 표현(소문자,float)    | printf(“%f”,1.2);  |       1.200000       |
  |     %F      |    실수의 소수점 표현(대문자,float)    | printf(“%f”,1.2);  |       1.200000       |
  |     %e      |    실수의 지수 표현법(소문자,float)    | printf(“%e”,1.2);  |     1.200000e+00     |
  |     %E      |    실수의 지수 표현법(대문자,float)    | printf(“%E”,1.2);  |     1.200000E+00     |
  |     %g      | %f 와 %e 중에서 짧은 것을 표현(소문자) | printf(“%g”,1,2);  |         1.2          |
  |     %G      | %F 와 %E 중에서 짧은 것을 표현(대문자) | printf(“%G”,1.2);  |         1.2          |
  |     %a      |     실수를 16진법으로 표현(소문자)     | printf(“%a”,1.2);  | 0x1.3333333333333p+0 |
  |     %A      |     실수를 16진법으로 표현(대문자)     | printf(“%A”,1.2);  | 0X1.3333333333333P+0 |
  |     %lf     |   실수의 소수점 표현(소문자,double)    | printf(“%lf”,1.2); |       1.200000       |
  |     %lF     |   실수의 소수점 표현(대문자,double)    | printf(“%lF”,1.2); |       1.200000       |
  |     %le     |   실수의 지수 표현법(소문자,double)    | printf(“%le”,1.2); |     1.200000e+00     |
  |     %lE     |   실수의 지수 표현법(대문자,double)    | printf(“%lE”,1.2); |     1.200000E+00     |

- 문자형

  | 형식 지정자 |  의미  |            예시             |  실행 결과  |
  | :---------: | :----: | :-------------------------: | :---------: |
  |     %c      |  문자  |      printf(“%c”,’A’);      |      A      |
  |     %s      | 문자열 | printf(“%s”,”Test String”); | Test String |

- 포인터

  | 형식 지정자 |         의미         |
  | :---------: | :------------------: |
  |     %p      | 포인터의 메모리 주소 |

- %기호

  > %를 출력하고 싶을때는 % 뒤에 %를 붙여 사용 `%%`

  ``` c
  printf("%%\n");
  // %
  ```

#### 플래그

| 플래그 | 설명                                                         |
| ------ | ------------------------------------------------------------ |
| -      | 왼쪽 정렬                                                    |
| +      | 양수일 때는 + 부호, 음수일 때는 - 부호 출력                  |
| 공백   | 양수일 때는 부호를 출력하지 않고 공백으로 표시, 음수 일 때는 - 부호 출력 |
| #      | 진법에 맞게 숫자 앞에 0, 0x, 0X를 붙임                       |
| 0      | 출력하는 폭의 남는 공간에 0으로 채움                         |

#### 너비

| 폭   | 설명                                                         |
| ---- | ------------------------------------------------------------ |
| 숫자 | 지정한 숫자만큼 폭을 지정하여 출력, 실수는 . (점), e+까지 폭에 포함됨 |

#### 정밀도

| 정밀도 | 설명                                  |
| ------ | ------------------------------------- |
| .숫자  | 지정한 숫자만큼 소수점 아래 자리 출력 |

