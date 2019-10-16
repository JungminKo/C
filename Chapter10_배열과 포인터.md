## 10-1. 배열과 포인터의 관계
#### 배열명으로 배열 요소 사용하기
- 주소 + 정수 -> 주소 + (정수 * 주소를 구한 변수의 크기)
- ary[1] ⇔ *(ary+1)

````C
#include <stdio.h>

int main(void) {

	int ary[3];
	int i;

	*(ary + 0) = 10;                                  // ary[0]=10
	*(ary + 1) = *(ary + 0) + 10;               // ary[1]=ary[0]+10

	printf("세 번째 배열 요소에 키보드 입력 : ");
	scanf_s("%d", ary + 2);                      // &ary[2]

	for (i = 0; i < 3; i++) {                          // 모든 배열 요소 출력
		printf("%5d", *(ary + i));         // ary[i]
	}

	return 0;
}
````
````
세 번째 배열 요소에 키보드 입력 : 30
   10   20   30
````
#### 배열명 역할을 하는 포인터
- 배열명은 주소이므로 포인터에 저장 가능.
- 이 경우, 포인터로도 연산식이나 대괄호를 써서 배열 요소를 쉽게 사용할 수 있음

````C
#include <stdio.h>

int main(void) {

	int ary[3];                             // 배열 선언
	int *pa = ary;                       // 포인터에 배열명 저장
	int i;                                     // 반복 제어 변수

	*pa = 10;                             // 첫 번째 배열 요소에 10 대입
	*(pa + 1) = 20;                    // 두 번째 배열 요소에 20 대입
	pa[2] = pa[0] + pa[1];          // 대괄호를 써서 pa를 배열명처럼 사용

	for (i = 0; i < 3; i++) {
		printf("%5d", pa[i]);
	}

	return 0;
}
````
````
   10   20   30
````

#### 배열명과 포인터의 차이
- 차이점 1 : sizeof 연산의 결과가 다름
<br>배열명 - sizeof : 배열 전체의 크기 구함
<br>포인터 - sizeof : 포인터 하나의 크기 구함

- 차이점 2 : 변수와 상수의 차이가 있음

포인터는 그 값을 바꿀 수 있지만, 배열명은 상수이므로 값을 바꿀 수 없음.

````C

#include <stdio.h>

int main(void) {

	int ary[3] = { 10, 20, 30 };
	int *pa = ary;
	int i;

	printf("배열의 값 : ");
	for (i = 0; i < 3; i++) {
		printf("%5d", *pa);         // pa가 가리키는 배열 요소 출력
		pa++;                            // 다음 배열 요소를 가리키도록 pa값 증가
	}

	return 0;
}
````
````
배열의 값 :    10   20   30
````

- 포인터 pa로 첫 번째 배열 요소를 출력하는 방법
    - 방법 1) pa를 배열명처럼 사용하여 첫 번째 배열 요소를 출력하는 방법
<br>printf(“%d”, pa[0])

    - 방법 2) pa[0]를 그대로 포인터 연산식으로 바꾸는 방법
<br>printf(“%d”, *(pa+0))

    - 방법 3) *(pa+0)에서 의미 없는 0과 괄호를 제거한 표현 방법
<br>printf(“%d”, *pa)


- 포인터로 배열의 데이터를 처리할 때 주의할 점
    <br>1) 포인터의 값이 변할 수 있으므로 유효한 값인지 확인
        <br>처음에는 포인터의 값이 배열을 가리키고 있었으나, pa++을 사용하면 값이 변화함
    <br>2) 포인터에 증가 연산자와 간접 참조 연산자를 함께 사용할 때 전위 표현을 사용하면 안됨
        <br>두번째 배열 요소부터 출력되며, 마지막에는 배열의 값이 아닌 쓰레기 값이 출력됨
        
#### 포인터의 뺄셈과 관계 연산
<가리키는 자료형이 같을 때>
- 포인터 - 포인터 -> 값의 차/가리키는 자료형의 크기

````C
#include <stdio.h>

int main(void) {

	int ary[5] = { 10, 20, 30, 40, 50};
	int *pa = ary;                                                   // 첫 번째 배열 요소 주소
	int *pb = pa + 3;                                              // 네 번째 배열 요소 주소 

	printf("pa : %u\n", pa);
	printf("pb : %u\n", pb);
	pa++;                                                              // pa를 다음 배열 요소로 이동
	printf("pb - pa : %u\n", pb - pa);                      // 두 포인터의 뺄셈

	printf("앞에 있는 배열 요소의 값 출력 : ");
	if (pa < pb) {
		printf("%d\n", *pa);                              // pa가 배열의 앞에 있으면 *pa 출력
	}
	else {
		printf("%d\n", *pb);                              // pb가 배열의 앞에 있으면 *pb 출력
	}

	return 0;
}
````
````
pa : 17692772
pb : 17692784
pb - pa : 2
앞에 있는 배열 요소의 값 출력 : 20
````

## 10-2. 배열을 처리하는 함수
#### 배열의 값을 출력하는 함수
- 배열을 처리하는 함수에 필요한 것은 배열의 주소
````C
#include <stdio.h>

void print_ary(int *pa);                                    // 함수 선언

int main(void) {

	int ary[5] = { 10, 20, 30, 40, 50};

	print_ary(ary);                                     // 배열명을 주고 함수 호출

	return 0;
}

void print_ary(int *pa) {                                   // 매개변수로 포인터 선언
	int i;

	for (i = 0; i < 5; i++) {
		printf("%d ", pa[i]);                   // pa로 배열 요소 표현식 사용
	}
}
````
````C
10 20 30 40 50
````
#### 배열 요소의 개수가 다른 배열도 출력하는 함수
- ary2의 size 대신 sizeof(ary2)/sizeof(ary2[0]) 써도 됨
````C
#include <stdio.h>

void print_ary(int *pa, int size);

int main(void) {

	int ary1[5] = { 10, 20, 30, 40, 50};                 // 배열 요소의 개수가 5개인 배열
	int ary2[7] = { 10, 20, 30, 40, 50, 60, 70 };    // 요소의 개수가 7개인 배열 

	print_ary(ary1, 5);                                     // ary1 배열 출력, 배열 요소의 개수 전달
	printf("\n");
	print_ary(ary2, 7);                                     // ary2 배열 출력, 배열 요소의 개수 전달

	return 0;
}

void print_ary(int *pa, int size) {             // 배열명과 배열 요소의 개수를 받는 매개변수 선언
	int i;

	for (i = 0; i < size; i++) {             // size의 값에 따라 반복 횟수 결정
		printf("%d ", pa[i]);
	}
}
````
````C
10 20 30 40 50
10 20 30 40 50 60 70
````
#### 배열에 값을 입력하는 함수
- 실수 배열에 값을 입력하는 함수와 최댓값을 찾는 함수
````C
#include <stdio.h>

void input_ary(double *pa, int size);
double find_max(double *pa, int size);

int main(void) {

	double ary[5];
	double max;
	int size = sizeof(ary) / sizeof(ary[0]);

	input_ary(ary, size);
	max = find_max(ary, size); 
	printf("배열의 최댓값 : %.1lf\n", max);

	return 0;
}

void input_ary(double *pa, int size) {
	int i;

	printf("%d개의 실수값 입력 : ", size);
	for (i = 0; i < size; i++) {
		scanf_s("%lf", pa + i, sizeof(pa + i)); 
	}
}

double find_max(double *pa, int size) {
	double max;
	int i;

	max = pa[0];
	for (i = 1; i < size; i++) {
		if (pa[i] > max) {
			max = pa[i];
		}
	}

	return max;
}
````
````C
5개의 실수값 입력 : 3.4 0.5 1.7 5.2 2.0
배열의 최댓값 : 5.2
````

## 도전 실전 예제 : 로또 번호 생성 프로그램
1~45 중에 6개의 서로 다른 수를 배열에 입력하고 출력합니다. 입력한 수가 이미 저장된 수와 같으면 에러 메시지를 출력하고 다시 입력합니다.다음 함수의 선언과 정의를 참고하고 빈 부분을 채워 완성합니다.

1) 번호를 받음
2) 1~45사이의 숫자가 아니면 다시 번호를 받음
3) 중복된 번호이면 다시 번호를 받음
````C
#include <stdio.h>

void input_nums(int *lotto_nums);
void print_nums(int *lotto_nums);

int main(void) {

	int lotto_nums[6];

	input_nums(lotto_nums);
	print_nums(lotto_nums);

	return 0;
}

void input_nums(int *lotto_nums) {
	
	int i, j;

	for (i = 0; i < 6; i++) {
		printf("번호 입력 : ");
		scanf_s("%d", lotto_nums + i, sizeof(lotto_nums + i));
		if ((*(lotto_nums + i) > 45) | (*(lotto_nums + i) < 0)) {
			printf("1부터 45사이의 숫자로 입력해주세요.\n");
			i = i - 1;
		}
		for (j = 0; j < i; j++) {
			if (*(lotto_nums + i) == *(lotto_nums + j)) {
				printf("같은 번호가 있습니다!\n");
				i = i - 1;
			}		
		}
	}
}

void print_nums(int *lotto_nums) {
	int i;

	printf("로또 번호 : ");
	for (i = 0; i < 6; i++) {
		printf("%d ",*(lotto_nums+i));
	}
}
````
````C
번호 입력 : 3
번호 입력 : 7
번호 입력 : 15
번호 입력 : 3
같은 번호가 있습니다!
번호 입력 : 22
번호 입력 : 35
번호 입력 : 40
로또 번호 : 3 7 15 22 35 40
````
