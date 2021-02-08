# 구조체

> 구조체를 활용하여 자신만의 자료형을 만드는 방법



## Contents

- [개념](#개념)
- [구조체 사용](#구조체-사용)



### 개념

- 구조체란 여러 개의 변수를 묶어 **하나의 객체를 표현**하고자 할 때 구조체를 사용한다
- 캐릭터, 몬스터, 학생, 좌표등 다양한 객체를 모두 프로그래밍 언어를 이용해 표현할 수 있다



### 구조체 사용

#### 정의

- 기본적인 정의

  ``` c
  struct Student {
  	char studentId[10];
  	char name[10];
  	int grade;
  	char major[20];
  };
  ```

- 전역 변수를 사용하는 정의

  ``` c
  struct Student {
  	char studentId[10];
  	char name[10];
  	int grade;
  	char major[20];
  } std; // std로 접근 가능.
  ```

- `typedef`를 이용한 정의

  - `typedef`를 사용하여 **별칭을 붙여** 구조체를 정의하면 구조체를 선언할때 `struct`키워드를 생략할 수 있다

  ``` c
  #include <stdio.h>
  
  typedef struct Student { // 구조체 정의
  	char studentId[10];
  	char name[10];
  	int grade;
  	char major[20];
  } Student; // 구조체 후미에 입력한 별칭으로 사용이 가능
  
  int main(void) {
  	struct Student defaultStd; // 기본 구조체 선언
  	Student typedefStd; // typedef를 사용한 선언
      
      return 0;
  }
  ```

  

#### 초기화

- 구조체 변수의 선언과 동시에 초기화 할 수 있다

  ``` c
  // 초기화 값은 선언한 순서대로 넣어줘야함
  struct Student std = { "20113157", "양중창", 4, "컴퓨터공학과" };
  ```

  

#### 접근

- 기본 접근

  - 기본적으로 구조체 변수에 접근할 때는 **온점(`.`)**을 사용한다

  ``` c
  #include <stdio.h>
  #include <string.h>
  
  struct Student { // 구조체 정의
  	char studentId[10];
  	char name[10];
  	int grade;
  	char major[20];
  };
  
  int main(void) {
  
  	struct Student std; // 구조체 변수 선언
  
  	strcpy(std.studentId, "20113157");
  	strcpy(std.name, "양중창");
  	std.grade = 4;
  	strcpy(std.major, "컴퓨터 공학과");
      
      printf("학번: %s\n", std.studentId);
      printf("이름: %s\n", std.name);
      printf("학년: %d\n", std.grade);
      printf("학과: %s\n", std.major);
      
  	/*
  	실행 결과
  	학번: 20113157
  	이름: 양중창
  	학년: 4 학년
  	학과: 컴퓨터 공학과
  	*/
      
  	system("pause");
  	return 0;
  }
  ```

- 포인터 접근

  - 구조체가 포인터 변수로 사용되는경우 내부 변수에 접근할때 **화살표(`->`)**를 사용한다

  ``` c
  #include <stdio.h>
  #include <stdlib.h>
  #include <string.h>
  
  typedef struct Student { // 구조체 정의
  	char studentId[10];
  	char name[10];
  	int grade;
  	char major[20];
  } Student;
  
  int main(void) {
  
  	Student* std = malloc(sizeof(Student)); // 구조체 포인터 변수 동적 할당
  	strcpy(std->studentId, "201103157"); // 구조체 포인터 변수에 접근
  	strcpy(std->name, "양중창");
  	std->grade = 4;
  	strcpy(std->major, "컴퓨터 공학과");
  
  	printf("학번: %s\n", std->studentId);
  	printf("이름: %s\n", std->name);
  	printf("학년: %d\n", std->grade);
  	printf("학과: %s\n", std->major);
  	
  	/*
  	실행 결과
  	학번: 201103157
  	이름: 양중창
  	학년: 4
  	학과: 컴퓨터 공학과
  	*/
  ```

  