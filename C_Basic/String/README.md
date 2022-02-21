# 문자열

> 문자열의 개념과 사용



## Contents

- [개념](#개념)
- [문자열의 기본 사용](#문자열의-기본-사용)
- [문자열 처리를 위한 다양한 함수](#문자열-처리를-위한-다양한-함수)



### 개념

- **문자열**은 말 그대로 문자들의 배열이다

- **문자열**은 컴퓨터 메모리 구조상에서 마지막에**NULL** 값을 포함한다

- 메모리 상의 **문자열** 구조의 예시

  - **NULL** 값은 문자열의 끝을 알리는 목적으로 사용된다
  - `printf()` 함수를 실행하면 컴퓨터 내부적으로 **NULL**을 만날 때까지 한글자씩 출력한다

  |  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |  10  |       11        |
  | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :-------------: |
  | 'H'  | 'E'  | 'L'  | 'L'  | 'O'  | ' '  | 'W'  | 'O'  | 'R'  | 'L'  | 'D'  | `\0` **(NULL)** |

  

### 문자열의 기본 사용

- 문자열의 선언

  - 문자열은 **배열**이므로 포인터 형태로 사용할 수있다

  ``` c
  char *someString = "Hello World"; 
  // "Hello World" 문자열의 메모리 주소를 someString에 할당
  
  printf("%s\n", someString); // Hello World
  
  /*
  이러한 문자열을 '문자열 리터럴' 이라고 말한다.
  이는 컴파일러가 알아서 메모리 주소를 결정함
  */
  ```

  

- 문자열은 **'문자'** 의 배열이기 때문에 직접 인덱스 접근도 가능하다

  ``` c
  char* someString = "Hello World"; 
  printf("%c\n", someString[1]); // e
  printf("%c\n", someString[4]); // o
  printf("%c\n", someString[8]); // r
  ```

- 문자열 입출력 함수

  - `gets()` : `scanf()` 함수는 공백을 만날때까지 입력 받지만 `gets()` 함수는 **공백까지** 포함하여 **한 줄을** 입력 받는다

    - 버퍼의 크기를 벗어나도 입력을 받아버리기 때문에 문제가 있어 사용되지 않음

    ``` c
    char someString[100]; // 100 크기의 문자 배열을 선언하여 입력 받을 준비를 한다
    gets(someString); // 입력: Hellow World !!
    printf("%s\n", someString); // Hellow World !!
    ```

  - `gets_s(value, size)`

    - **C11** 표준부터는 버퍼의 크기를 철저히 지키는 `gets_s()` 함수가 추가됨
    - 버퍼 범위를 넘으면 그 즉시 런타임 오류가 발생하게 된다

    

    ``` c
    char someString[100]; // 100 크기의 문자 배열을 선언하여 입력 받을 준비를 한다
    
    gets_s(someString, sizeof(someString)); // 입력 : Hellow World !!
    /* 
    sizeof() -> 자료의 크기를 바이트 단위로 환산하여 반환 하는 함수
    someString은 100 크기의 Character 배열이기 때문에 1Byte * 100 = 100을 반환한다
    따라서 위에서 사용된 gets_s() 함수에서 입력 받을 수 있는 문자는 100개이다
    */
    printf("%s\n", someString); // Hellow World !!
    ```



### 문자열 처리를 위한 다양한 함수

> C언어에서의 문자열 함수는 `<string.h>` 라이브러리에 포함되어 있음 따라서



- `strlen()` : 문자열의 길이를 반환

  ``` c
  #include <stdio.h>
  #include <string.h> 
  // 원래는 include를 필수적으로 해야 하지만 Visual Studio에는 기본적으로 포함하고 있어서 include 하지 않아도 됨
  
  int main(void) {
      
  	char name[20] = "JoongChang Yang"; 
      
  	printf("문자열의 길이: %d\n", strlen(name));
  	// 문자열의 길이: 15
  
  	system("pause");
  	return 0;
  }
  ```

- `strcmp(string1, string2)` : `string1` 이 `string2` 보다 사전적으로 앞에있으면 `-1`, 뒤에있으면 `1`을, 완전히 같다면 `0` 반환

  ``` c
  char myName[20] = "Aiden"; 
  char friendName[20] = "Hash";
  
  printf("두 문자열의 사전순 비교: %d\n", strcmp(myName, friendName));
  // 두 문자열의 사전순 비교: -1
  ```

- `strcpy(string1, string2)` : `string1`에 `string2`를 복사하여 할당

  ``` c
  char myName[20] = "Aiden"; 
  char friendName[20] = "Hash";
  
  strcpy(myName, friendName); // myName에 friendName을 복사
  
  printf("복사된 문자열: %s\n", myName);
  // 복사된 문자열: Hash
  ```

- `srecat(string1, string2)` : `string1`뒤에 `string2`를 합한 문자열을 `string1`에 할당

  - 결과값을 할당 받을 변수의 배열 크기에 신경 써야함

  ``` c
  char myName[40] = "Aiden"; // friendName이 합쳐진 문자열을 할당할것이기 때문에 크기를 더줌
  char friendName[20] = "Hash";
  strcat(myName, friendName); // myName + friendName
  printf("합쳐진 결과 문자열: %s\n", myName);
  // 합쳐진 결과 문자열: AidenHash
  ```

- `strstr(string1, string2)` : `string1`에 `string2`가 포함되어있는지 여부를 판단

  - 포함한다면 문자열을 찾은 주소 값 자체를 반환, 포함하지 않는다면 **NULL**을 반환

  ``` c
  char baseString[20] = "I like you"; 
  char findString[20] = "like";
  
  printf("찾은 문자열: %s\n", strstr(baseString, findString));
  /*
  찾은 문자열: like you
  like를 찾은 주소 값 자체를 반환하므로 "I like you" 에서 "like" 부터 뒤로 쭉 출력 -> "like you"
  */
  ```
  
- `strtok(string, separator)` : `string` 을 `separator` 를 기준으로 자르는 함수

  - 문자열을 자르고 그 잘라진 문자열의 포인터를 반환한다

  ``` c
  char hi[100] = "Hi my name is Joongchang Yang";
  
  // hi 문자열을 " " 기준으로 잘라서 잘린 문자열의 포인터를 반환
  char* ptr = strtok(hi, " ");
  
  while (ptr != NULL) {
      printf("자른 문자열 : %s\n", ptr);
      printf("자른 문자열 포인터 : %d\n", ptr);
      printf("자른 문자열 길이 : %d\n", strlen(ptr));
      printf("\n");
      
      // NULL을 넣고 strtok를 해주어야 다음 잘린 문자열 포인터로 이동할 수 있다
      ptr = strtok(NULL, " ");
  }
  
  /*
  실행 결과
  
  자른 문자열 : Hi
  자른 문자열 포인터 : 18414040
  자른 문자열 길이 : 2
  
  자른 문자열 : my
  자른 문자열 포인터 : 18414043
  자른 문자열 길이 : 2
  
  자른 문자열 : name
  자른 문자열 포인터 : 18414046
  자른 문자열 길이 : 4
  
  자른 문자열 : is
  자른 문자열 포인터 : 18414051
  자른 문자열 길이 : 2
  
  자른 문자열 : Joongchang
  자른 문자열 포인터 : 18414054
  자른 문자열 길이 : 10
  
  자른 문자열 : Yang
  자른 문자열 포인터 : 18414065
  자른 문자열 길이 : 4
  */
  ```

  
