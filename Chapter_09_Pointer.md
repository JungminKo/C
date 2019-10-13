## 09-1. 포인터의 기본 개념

#### 주소 연산자 : `&`

- & + 변수이름 : 변수의 시작 주소 반환

- 주소는 보통 16진수 표기 -> 주소를 출력할 때에는 %p를 사용하는 것이 좋음
<br>(%p : 주소값의 데이터 크기에 따라 자릿수를 맞춰 16진수 대문자로 출력)
<br>(%u : 주소는 음수가 없으므로 편의상 10진수로도 출력할 수 있음)

```C
#include <stdio.h>

int main(void) {

	int a;
	double b;
	char c;

	printf("int형 변수의 주소 \n 16진수 : %p, 10진수 : %u \n\n", &a, &a);
	printf("double형 변수의 주소 \n 16진수 : %p, 10진수 : %u \n\n", &b, &b);
	printf("char형 변수의 주소 \n 16진수 : %p, 10진수 : %u \n\n", &c, &c);

	return 0;
}
```
```
int형 변수의 주소
 16진수 : 00DBF83C, 10진수 : 14415932

double형 변수의 주소
 16진수 : 00DBF82C, 10진수 : 14415916

char형 변수의 주소
 16진수 : 00DBF823, 10진수 : 14415907
```

#### 포인터와 간접 참조 연산자 : *

- 포인터 자체가 변수의 메모리 주소를 저장하는 변수가 될 수 있음 

- 일반 변수와 동일하게, 선언 후에 사용 가능

- \* : 간접참조연산자(또는 포인터 연산자) ; 포인터가 가리키는 변수를 사용할 때 사용

```C
#include <stdio.h>

int main(void) {

	int a; // 일반 변수 선언
	int *pa; // 포인터 선언 // int값은 주소 위치에 있는 변수의 자료형

	pa = &a; // 포인터에 a의 주소 대입
	*pa = 10; // 포인터로 변수 a에 10 대입

	printf("포인터로 a 값 출력 : %d\n", *pa);
	printf("변수명으로 a 값 출력 : %d\n", a); //변수 a 값 출력

	return 0;
}
```
```
포인터로 a 값 출력 : 10
변수명으로 a 값 출력 : 10
```

#### 여러 가지 포인터 사용해보기
```C
#include <stdio.h>

int main(void) {

	int a=10, b=15, total;      // 변수 선언과 초기화
	double avg;                    // 평균을 저장할 변수
	int *pa, *pb;                    // 포인터 동시 선언
	int *pt = &total;               // 포인터 선언과 초기화
	double *pg = &avg;        // double형 포인터 선언과 초기화

	pa = &a;                         // 포인터 pa에 변수 a의 주소 대입
	pb = &b;                         // 포인터 pb에 변수 b의 주소 대입

	*pt = *pa + *pb;              // a 값과 b 값을 더해 total에 저장
	*pg = *pt / 2.0;              // total 값을 2로 나눈 값을 avg에 저장

	printf("두 정수의 값 : %d, %d\n", *pa, *pb);           // a 값과 b 값 출력
	printf("두 정수의 합 : %d\n", *pt);                          // total 값 출력
	printf("두 정수의 평균 : %.1lf\n", *pg);                  // avg 값 출력

	return 0;
}
```
```
두 정수의 값 : 10, 15
두 정수의 합 : 25
두 정수의 평균 : 12.5
```
#### const를 사용한 포인터

- const의 의미 : pa가 가리키는 변수 a는 pa를 간접 참조하여 바꿀 수 없다는 것
<br>주의 ) 변수 a 자체를 사용하여 값을 바꾸는 것은 가능함


- 언제 사용?	
	<br>- 문자열 상수를 인수로 받는 함수에서 사용
	<br>(문자열 상수는 값이 바뀌면 안되는 저장 공간이므로 매개변수를 통해서 값을 바꿀 수 없도록 매개변수로 선언된 포인터에 const를 사용)

```C
#include <stdio.h>

int main(void) {

	int a = 10, b = 20;     
	const int *pa = &a;                              // 포인터 pa는 변수 a를 가리킨다.

	printf("변수 a 값 : %d\n", *pa);            // 포인터를 간접 참조하여 a 출력
	pa = &b;                                              // 포인터가 변수 b를 가리키게 한다..
	printf("변수 b 값 : %d\n", *pa);            // 포인터를 간접 참조하여 b 출력
	pa = &a;                                              // 포인터가 다시 변수 a를 가리킨다.
	a = 20;                                                // a를 직접 참조하여 값을 바꾼다.
	printf("변수 a 값 : %d\n", *pa);            // 포인터를 간접 참조하여 바뀐 값 출력

	return 0;
}
```
```
변수 a 값 : 10
변수 b 값 : 20
변수 a 값 : 20
```

## 09-2. 포인터 완전 정복을 위한 포인터 이해하기
#### 주소와 포인터의 크기
- sizeof 연산자로 주소와 포인터의 크기 확인 가능

```C
#include <stdio.h>

int main(void) {

	char ch;
	int in;
	double db;

	char *pc = &ch;
	int *pi = &in;
	double *pd = &db;

	printf("char형 변수의 주소 크기 : %d\n", sizeof(&ch));
	printf("int형 변수의 주소 크기 : %d\n", sizeof(&in));
	printf("double형 변수의 주소 크기 : %d\n\n", sizeof(&db));

	printf("char * 포인터의 크기 : %d\n", sizeof(pc));
	printf("int * 포인터의 크기 : %d\n", sizeof(pi));
	printf("double * 포인터의 크기 : %d\n\n", sizeof(pd));

	printf("char * 포인터가 가리키는 변수의 크기 : %d\n", sizeof(*pc));
	printf("int * 포인터가 가리키는 변수의 크기 : %d\n", sizeof(*pi));
	printf("double * 포인터가 가리키는 변수의 크기 : %d\n\n", sizeof(*pd));

	return 0;
}
```
```
char형 변수의 주소 크기 : 4               // 32바이트 주소 체계의 프로그램을 사용하므로
int형 변수의 주소 크기 : 4
double형 변수의 주소 크기 : 4

char * 포인터의 크기 : 4
int * 포인터의 크기 : 4
double * 포인터의 크기 : 4

char * 포인터가 가리키는 변수의 크기 : 1
int * 포인터가 가리키는 변수의 크기 : 4
double * 포인터가 가리키는 변수의 크기 : 8
```
#### 포인터의 대입 규칙
규칙 1) 포인터는 가리키는 변수의 형태가 같을 때만 대입해야 한다.
```C
#include <stdio.h>

int main(void) {

	int a = 10;                  // 변수 선언과 초기화
	int *p = &a;                // 포인터 선언과 동시에 a를 가리키도록 초기화
	double *pd;               // double형 변수를 가리키는 포인터

	pd = p;                       // 포인터 p 값을포인터 pd에 대입
	printf("%lf\n", *pd);     // pd가 가리키는 변수의 값 출력

	return 0;
}
```
규칙 2) 형 변환을 사용한 포인터의 대입은 언제나 가능하다.
```C
double a = 3.4;            // double형 변수 선언
double *pd = &a;         // pd가 double형 변수 a를 가리키도록 초기화
int *pi;                          // int형 변수를 가리키는 포인터 
pi = (int *)pd;                // pd값을 형 변환하여 pi에 대입
```

#### 두 변수의 값을 바꾸며 포인터 이해하기
변수의 값을 바꾸는 함수(swap 함수)의 세가지 step
1) 첫번째 변수의 값을 임시 변수의 값에 복사하기
2) 두번째 변수의 값을 첫번째 변수의 값에 복사하기
3) 임시 변수의 값(즉, 첫번째 변수의 값)을 두번째 변수의 값에 복사하기

```C
#include <stdio.h>

void swap(int *pa, int *pb);                       // 두 변수의 값을 바꾸는 함수의 선언

int main(void) {

	int a = 10, b = 20;                         // 변수의 선언과 초기화

	swap(&a, &b);                              // a, b 의 주소를 인수로 주고 함수 호출
	printf("a:%d, b:%d\n", a, b);         // 변수 a, b 출력

	return 0;
}

void swap(int *pa, int *pb) {               // 매개변수로 포인터 선언

	int temp;                                // 교환을 위한 임시 변수

	temp = *pa;                           // temp에 pa가 가리키는 변수의 값 저장
	*pa = *pb;                             // pa가 가리키는 변수에 pb가 가리키는 변수의 값 저장
	*pb = temp;                          // pb가 가리키는 변수에 temp 값 저장
}
```
```
a:20, b:10
```
그렇지만, 포인터를 사용하지 않고 두 변수의 값을 바꾸는 함수는 불가능할까?

void함수를 사용하면 답은 그렇다!
BUT , main함수에서 a, b의 값을 swap함수에 인수로 주면 가능해짐!

```C
#include <stdio.h>

void swap(int x, int y);                                // 두 변수의 값을 바꾸는 함수의 선언

int main(void) {

	int a = 10, b = 20;                          // 변수의 선언과 초기화

	swap(a, b);                                    // a, b의 값을 복사해서 전달
	printf("a:%d, b:%d\n", a, b);          // 변수 a, b 출력

	return 0;
}

void swap(int x, int y) {                             // 인수 a, b 를 x, y에 복사해서 저장
 
	int temp;                                       // 교환을 위한 변수

	temp = x;                                      // temp에 x 값 저장
	x = y;                                            // x에 y 값 저장
	y = temp;                                      // y에 temp 값 저장
}
```
```
a:20, b:10
```

#### 도전 실전 예제
- 다음코드는 키보드로 실수 3개를 입력한 다음 큰 숫자부터 작은 숫자로 정렬한 뒤 출력하는 프로그램
- 이미 정해진 swap함수를 이용하여 line_up 함수 작성하기
```C
#include <stdio.h>

void swap(double *pa, double *pb);
void line_up(double *maxp, double *midp, double *minp);

int main(void) {

	double max, mid, min;

	printf("실수값 3개 입력 : ");
	scanf_s("%lf%lf%lf", &max, &mid, &min);
	line_up(&max, &mid, &min);
	printf("정렬된 값 출력 : %.1lf, %.1lf, %.1lf\n", max, mid, min);
	return 0;
}


void swap(double *pa, double *pb) {

	double temp;

	temp = *pa;
	*pa = *pb;
	*pb = temp;
}

void line_up(double *maxp, double *midp, double *minp) {

	if (*minp > *midp) {
		swap(midp, minp);
	}
	if (*midp > *maxp) {
		swap(maxp, midp);
	}
	if (*minp > *midp) {
		swap(midp, minp);
	}
	
}
```
```
실수값 3개 입력 : 2.7 1.5 3.4
정렬된 값 출력 : 3.4, 2.7, 1.5
```
