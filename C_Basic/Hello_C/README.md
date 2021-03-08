# Hello C

- `#include` 명령어를 통해 다양한 라이브러리를 불러올 수 있다.
  - 대표적으로 `stdio.h`는 여러 기본적인 기능(입출력 등)을 담고있다.
  - `main`함수로부터 시작된다.
  - `system()` 함수를 이용하여 운영체제의 기본적인 기능을 이용할 수 있다.

``` c
#include <stdio.h>

int main(void) {
	printf("HELLO WOLD\n");
	system("pause");
	return 0;
}
```

