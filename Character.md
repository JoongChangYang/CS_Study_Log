# 문자

> C 언어에서의 문자 개념



## Contents

- [아스키 코드](#아스키-코드)
- [버퍼](#버퍼)



### 아스키 코드

- C 프로그램의 문자는 기본적으로 **아스키 코드(Ascii Code)** 를 따른다
- 아스키 코드는 0~127중의 1바이트로 구성되며 주요 문자를 출력하도록 해준다

|           문자           | 아스키 코드 |
| :----------------------: | :---------: |
|     숫자('0' ~ '9')      |   48 ~ 57   |
| 대문자 알파벳('A' ~ 'Z') |   65 ~ 90   |
| 소문자 알파벳('a' ~ 'z') |  97 ~ 122   |

- 아스키 코드를 이용하여 바로 문자를 출력할 수도 있다.

  ``` c
  char a = 65;
  printf("%c\n", a); // A
  ```

- `getchar()` 함수를 통하여 한개의 문자를 사용자로부터 입력 받을 수 있다.

  ``` c
  char character = getchar(); // 입력: c
  printf("%c\n"m cgaracter) // c
  ```

  

### 버퍼

- 문자열을 처리할 때 버퍼의 개념이 많이 사용됨.

- **버퍼 (Buffer)** 란 임시적으로 특정 데이터를 저장하기 위한 목적으로 사용된다.

- C 프로그램은 기본적으로 사용자가 의도하지 않아도 자동으로 버퍼를 이용해 입출력을 처리한다.

- 버퍼로 인한 오류 예시

  ``` c
  int someNumber;
  char someChar;
  
  scanf("%d", &someNumber); // 입력: 800 -> Enter
  printf("%d\n", someNumber); // 800
  /*======================프로그램 종료======================*/
  scanf("%c", &someChar);
  scanf("%c\n", someChar);
  /*
   최초 someNumber에 입력을 받을때 입력한 숫자 뿐 아니라 Enter가 줄바꿈(\n)으로 버퍼에 남아 다음 someChar에 '\n'이 담겨 의도와는 다르게 동작
   */
  ```

- 입력 버퍼를 비워 오류를 제거한 예시

  ``` c
  int someNumber;
  char someChar;
  
  scanf("%d", &someNumber); // 입력: 800 -> Enter
  printf("%d\n", someNumber); // 800
  
  // 한 자씩 받아서 파일의 끝의 개행문자를 만나면 입력을 멈추므로 항상 입력 버퍼를 비운다
  // EOF -> End Of File의 약자로 파일의 끝(파일을 읽는다고 가정했을때)
  int temp; // 문자도 내부적으로 숫자이기 때문에 int 선언으로도 getchar()가 가능함
  while ((temp = getchar()) != EOF && temp != '\n') {}
  
  scanf("%c", &someChar); // 입력: c
  scanf("%c\n", someChar); // c
  ```

  

