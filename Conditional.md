# 조건문

## Contents

- [if](#if)
- [switch](#switch)



### if

- 내부의 조건을 검사해 프로그램의 진행 경로를 결정.
- if문은 조건의 개수가 많지 않을때 사용하는 것이 유리함.

``` c
if (조건 1) {
    // 조건 1에 부합할 때
} else if (조건 2) {
    // 조건 1에 부합하지 않지만 조건 2에 부합할 때
} else {
    // 위 조건들에 모두 부합하지 않을때
}
```



### switch

- 다양한 조건이 존재할때 사용하면 소스코드를 짧게 유지할 수 있음.

``` c
switch (확인 대상) {
    case 값1:
        // 값 1에 부합할 때
    case 값2:
        // 값 2에 부합할 때
	default:
        // 모든 경우
}
```

- **switch** 구문은 기본적으로 만족하는 **case**를 만나면 그 하단의 **case**들을 모두 만족시킨다.

- 특정 **case**를 만족했을때 하단의 **case** 들을 수행하고싶지 않다면 **break**을 넣어 **switch** 구문을 빠져나와야 한다.

  - 나쁜 예

    ``` c
    char grades;
    
    printf("학점을 입력하세요. ");
    scanf("%c", &grades);
    
    switch (grades) {
    case 'A':
    	printf("A 학점입니다.\n");
    case 'B':
    	printf("B 학점입니다.\n");
    case 'C':
    	printf("C 학점입니다.\n");
    default:
    	printf("학점을 바르게 입력하세요.\n");
    }
    
    // 입력: 학점을 입력하세요. A
    // A 학점입니다.
    // B 학점입니다.
    // C 학점입니다.
    // 학점을 바르게 입력하세요.
    ```

  - 좋은 예

    ``` c
    // ex1)
    char grades;
    
    printf("학점을 입력하세요. ");
    scanf("%c", &grades);
    
    switch (grades) {
    case 'A':
    	printf("A 학점입니다.\n");
            break;
    case 'B':
    	printf("B 학점입니다.\n");
            break;
    case 'C':
    	printf("C 학점입니다.\n");
            break;
    default:
    	printf("학점을 바르게 입력하세요.\n");
    }
    
    // 학점을 입력하세요. B
    // B 학점입니다.
    
    // ex2)
    int month;
    
    printf("월을 입력하세요. ");
    scanf("%d", &month);
    
    switch (month) {
    case 1:
    case 2:
    case 3:
    	printf("봄\n");
    	break;
    case 4:
    case 5:
    case 6:
    	printf("여름\n");
    	break;
    case 7:
    case 8:
    case 9:
    	printf("가을\n");
    	break;
    case 10:
    case 11:
    case 12:
    	printf("겨울\n");
    	break;
    }
    // 입력: 월을 입력하세요. 5
    // 여름
    ```

    

