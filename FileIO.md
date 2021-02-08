# 파일 입출력

> 소프트웨어에서 파일 입출력이 필요한 이유에 대한 이해
>
> C언어를 이용해 파일 입출력을 하는 방법에 대해서 학습



## Contents

- [개념](#개념)
- [파일 열고 닫기](#파일-열고-닫기)
- [예제](#예제)



### 개념

#### 파일 입출력의 필요성

- 프로그램이 꺼진 이후에도 데이터를 저장하기 위해서 파일 입출력이 필요함
- 파일은 일반적으로 **보조 저장장치(SSD, HDD)**에 저장된다

### 파일 열고 닫기

> 파일을 열고 닫는 방법

#### 개요

- 파일 입출력 변수는 **FILE**형식의 포인터 변수로 선언한다
- 파일열기: `fopen(파일 경로, 접근 방식)`
- 파일 닫기: `fclose(오픈한 파일의 포인터)`

``` c
FILE* someFile; // 파일 포인터 선언
someFile = fopen(파일 경로, 접근방식); // 파일을 열고 그 파일에 접근할 수 있는 포인터를 반환

// 파일 관련 처리

fclose(someFile); // 파일 닫기
```

#### 파일 열기

- 파일 열기 함수인 `fopen()` 함수에는 **파일 경로**와 **접근 방식**을 설정할 수 있다

- 기본 경로는 현재 프로그램의 경로이다

- 접근 방식

  | 접근 방식 |                             설명                             |
  | :-------: | :----------------------------------------------------------: |
  |    `r`    |               파일에 접근하여 데이터를 읽는다                |
  |    `w`    | 파일에 접근하여 데이터를 기록한다 (파일이 이미 존재하면 덮어쓰기) |
  |    `a`    |         파일에 접근하여 데이터를 뒤에서부터 기록한다         |

#### 파일 입출력 함수

- 파일 입출력에서는 `fprintf()`와 `fscanf()`가 사용된다

  ``` c
  fprintf(파일 포인터, 서식, 형식지정자);
  fscanf(파일 포인터, 서식, 형식지정자);
  ```

#### 파일 입출력 과정

- 파일 입출력은 **열고**, **읽고/쓰고**, **닫기**의 과정을 철저히 따라야 한다

- 파일을 열 때는 **파일 포인터**가 사용되며, 이는 **동적**으로 할당된 것

- 따라서 파일 처리 이후에 파일을 닫아주지 않으면 메모리 누수가 발생함

- **[파일 열기] → [파일 읽기/쓰기] → [파일 닫기]**

- **기본 예제**

  ``` c
  char str[20] = "Hello World";
  FILE *fp;
  
  fp = fopen("Test.txt", "w"); // Test.txt 파일을 쓰기 용도로 연다(없을시 생성)
  
  fprintf(fp, "%s\n", str); // Test.txt 파일에 str("Hello Word)를 쓰기
  
  fclose(fp); // Test.txt 파일을 닫는다
  ```



### 예제

#### 간단한 학생 정보 시스템 만들기

- 학생들의 이름과 성적이 들어간 텍스트 파일을 읽어와서 보여주는 시스템

1. 학생들의 이름과 성적 파일 만들기

   > txt 파일을 만들때 UTF-8인코딩을 사용하니 한글이 깨져 ANSI를 쓰니 잘 나옴
   >
   > 경로 : **/[프로젝트폴더]/Debug/[파일]**

   ![File_IO_Example1](https://github.com/JoongChangYang/TIL_C/blob/main/Assets/File_IO_Example1.PNG)

2. 소스코드 작성

   ``` c
   #include <stdio.h>
   #include <stdlib.h>
   #include <string.h>
   
   typedef struct {
   	char name[10];
   	int score;
   } Student;
   
   int main(void) {
   
   	int numberOfStudents, totalScore = 0; // 총 학생의 수, 총 점수를 구할 변수 선언
   
   	FILE* fp; // 파일 포인터 선언
   	fp = fopen("ScoreList.txt", "r"); // 파일 읽기로 열기
   
   	// 읽어온 파일에서 최 상단 숫자 1개를 읽어 numberOfStudents에 할당
   	fscanf(fp, "%d", &numberOfStudents);
   
   	// 총 학생 수(numberOfStudents) 만큼 Student 포인터 변수에 메모리 동적 할당
   	Student* students = (Student*)malloc(sizeof(Student) * numberOfStudents);
   
   	for (int i = 0; i < numberOfStudents; i++) {
   		// 반복문을 돌면서 파일에 문자열과 정수를 한줄씩 읽어옴
   		fscanf(fp, "%s %d", &((students + i)->name), &((students + i)->score));
   
   		// 파일에서 읽어온 값들을 임시 변수에 할당
   		char tempName[10];
   		strcpy(name, (students + i)->name);
   		int temScore = (students + i)->score;
   
   		// 총 점수에 읽어온 점수를 더해줌
   		totalScore += temScore;
   
   		// 읽어온 문자열과 정수를 콘솔에 출력
   		printf("이름: %s, 성적%d\n", tempName, temScore);
   	}
   	
   	// students 동적 메모리 할당 해제
   	free(students);
   
   	// 총 점수 / 학생수 = 평균
   	double averageScore = (double)totalScore / numberOfStudents;
   	printf("총 학생 수 : %d\n점수 평균 : %.2f\n", numberOfStudents, averageScore);
   
   	fclose(fp); // 파일 닫기
   
   	/*
   	실
   	이름: 홍길동, 성적98
   	이름: 이순신, 성적84
   	이름: 유관순, 성적89
   	이름: 정약용, 성적95
   	이름: 신립, 성적97
   	총 학생 수 : 5
   	점수 평균 : 92.60
   	*/
   
   
   	system("pause");
   	return 0;
   }
   ```

3. **Debug** 응용 프로그램 생성

   1. **[솔루션 우클릭] → [솔루션 빌드]**
   2. 빌드 완료 후 **/프로젝트/Debug**로 이동후 **프로젝트.exe** 실행 후 확인 

   