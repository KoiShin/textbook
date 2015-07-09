
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

TODO:

- swap関数
- ソート関数
- 文字列を返す関数
- 構造体を受け取る関数
- ファイルの読み書き




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
printf("a is %d", a);  // a is 17
printf("b is %d", b);  // b is 4
~~~

[Answer](#ptr1)

## 課題2. ソート関数

int型の配列を引数とし、その配列を大きい順にソートする関数を作成せよ。

[Answer](#ptr2)

## 課題3. カンマ区切り

long int型の変数と文字列を引数とし、引数の文字列に3桁ずつカンマで区切った文字列のポインタを代入せよ。

Code Example:

~~~ c
void seperate(long int num, char *str){
	//
	// Execute some process
	//
	str = &seperated_str;
}
~~~

[Answer](#ptr3)

## 課題4. 構造体

構造体をポインタで受け取る関数...

## 課題5. ファイル操作











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
## 2. 
~~~ c

~~~

<a name="ptr3"></a>
## 3. 
~~~ c

~~~

<a name="ptr4"></a>
## 4. 
~~~ c

~~~

<a name="ptr5"></a>
## 5. 
~~~ c

~~~














