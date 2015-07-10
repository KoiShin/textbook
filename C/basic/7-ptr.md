
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# ポインタ(Pointer)

- 先修 : 構造体(Structure)
- 後修 : 

はじめに、ポインタのおさらい

~~~ c
int var = 20;
int *ptr;

// store address of var in pointer variable
ptr = &var;
~~~


## 課題1. swap関数

2つの変数の値の入れ替えは

~~~ c
int tmp = a;
a = b;
b = tmp;
~~~

で行うことができる。この一連の作業を関数swapとして作成せよ。

Code Example:

~~~ c
int a = 4, b = 17;
swap(&a, &b);
printf("a is %d\n", a);  // => a is 17
printf("b is %d\n", b);  // => b is 4
~~~

[Answer](#ptr1)

## 課題2. ソート関数

int型の配列を引数とし、その配列を大きい順にソートする関数を作成せよ。

Code Example:

~~~ c
int size = 10;
int array[] = {5,2,7,9,3,1,8,6,2,5};
sort(array, size);

// array will be :
//   {1,2,2,3,5,5,6,7,8,9}
~~~

[Answer](#ptr2)

## 課題3. 文字列

引数に与えた文字列を大文字にする uppercase() という関数を作成せよ。

Code Example:

~~~ c
char str[] = "String";
uppercase(str);
printf("%s\n", str);  // => STRING
~~~

[Answer](#ptr3)

## 課題4. 構造体

座標に関する構造体 Point を作成する。

2つの Point 型の変数を引数として、
二つの座標の和を Point 型の変数として返す関数 addPoint() を作成せよ。

Code Example:

~~~ c
typedef struct {
	double x, y;
} Point;

int main(void) {
	Point a = {2, 3};
	Point b = {4, 5};
	Point c;

	c = addPoint(&a, &b);

	printf("(%f, %f)\n", c.x, c.y);
	// => (6.0, 8.0)
}
~~~

[Answer](#ptr4)

## 課題5. ポインタのポインタ

関数の中から関数を呼ぶ際に、ポインタを引数として渡すにはどのようにすればよいか。  
コメントアウト `/* */` を適切な処理に書きかえよ。

~~~ c
typedef struct {
	double x, y;
} Point;

//
// main -> func1 -> func2 の順番で呼び出される
//

void func2(Point *p2) {
	/*
	 * 引数p2のx,yを出力する
	 */
}

void func1(Point *p1) {
	/*
	 * 引数p1を与えてfunc2を呼び出す
	 */
}

int main(void) {
	Point point = {1,2};
	func1(&point);
}
~~~




----

# ポインタ(Pointer) - 回答

<a name="ptr1"></a>
## 1. swap関数
~~~ c
#include <stdio.h>

void swap(int *a, int *b){
	int tmp = *a;
	*a = *b;
	*b = tmp;
}

int main(void){
	int a = 4, b = 17;
	swap(&a, &b);
	printf("a is %d\n", a);  // a is 17
	printf("b is %d\n", b);  // b is 4
}
~~~

<a name="ptr2"></a>
## 2. ソート関数
~~~ c
#include <stdio.h>

void swap(int *a, int *b) {
	int tmp = *a;
	*a = *b;
	*b = tmp;
}

void sort(int *array, int size) {
	int i, j;

	for (i = 0; i < size - 1; i++) {
		for (j = size - 1; j > i; j--) {
			if (array[j - 1] > array[j]) {
				swap(&array[j], &array[j - 1]);
			}
		}
	}
}

int main(void) {
	int i;
	int size = 10;
	int array[] = {5,2,7,9,3,1,8,6,2,5};
	sort(array, size);

	// show all elements in array
	for (i = 0; i < size; i++) {
		printf("%d,", array[i]);
	}
	printf("\n");
}
~~~

<a name="ptr3"></a>
## 3. 文字列
~~~ c
#include <stdio.h>

//
// 引数の文字列strを大文字にする
// *(str + i) でi番目の文字にアクセスできる
// 小文字から空白（文字コード：32）を引き算すると、大文字になる
// cf. ASCII文字コード表
//
void uppercase(char *str) {
	int i;
	for (i = 0; *(str + i) != '\0'; i++) {
		if ('a' <= *(str + i) && *(str + i) <= 'z') {
			*(str + i) -= ' ';
		}
	}
}

int main(void) {
	char str[] = "sample String";
	uppercase(str);
	printf("%s\n", str);  // => SAMPLE STRING
}
~~~

<a name="ptr4"></a>
## 4. 構造体
~~~ c
#include <stdio.h>

typedef struct {
	double x, y;
} Point;

Point addPoint(Point *p1, Point *p2) {
	Point result;
	result.x = p1->x + p2->x;
	result.y = p1->y + p2->y;
	return result;
}

int main(void) {
	Point a = {2, 3};
	Point b = {4, 5};
	Point c;

	c = addPoint(&a, &b);

	printf("(%f, %f)\n", c.x, c.y);
	// => (6.0, 8.0)
}
~~~

<a name="ptr5"></a>
## 5. ポインタのポインタ
~~~ c
#include <stdio.h>

void func2(Point *p2) {
	printf("(%f, %f)\n", p2->x, p2->y);
}

void func1(Point *p1) {
	func2(&*p1);
}

int main(void) {
	Point point = {1,2};
	func1(&point);
}
~~~














