
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# 関数(Function)

- 先修 : 配列(Array)
- 後修 : 構造体(Struct)

はじめに、関数のおさらい

~~~ c
// Defining a Function
return_type function_name(parameter){
	//
	// Execute some process
	//
	return variable;
}
~~~


## 課題1. 素数判定器

引数として整数を1つ取り、それが素数なら「1」、素数ではないなら「0」を返す関数を作成せよ。

さらに、1なら「_num_ is prime」、0なら「_num_ is not prime」と表示せよ。

Console:
	
	input a number > 97
	97 is prime

	input a number > 98
	98 is not prime

[Answer](#func1)

## 課題2. 曜日

西暦y年m月d日の曜日は次の式で求めることができる。

	(y + [y / 4] - [y / 100] + [y / 400] + [(13m + 8)/5] + d) mod 7

この式での`[]`は床関数（切り捨てを行う関数）とする。
この式の計算結果が0なら日曜日、1なら月曜日、...、6なら土曜日を表す。

西暦y年m月d日を関数の引数とし、曜日（3文字の英語）を出力する関数を作成せよ。

Code:

~~~ c
void day_of_week(int y, int m, int d){ /* 省略 */ };
~~~

Output:

	input year  > 2015
	input month > 4
	input day   > 1
	Wed

[Answer](#func2)

## 課題3. ユークリッドの互除法

ユークリッドの互除法の説明は次の通りである（wikipediaより引用）。

> ユークリッドの互除法とは2つの自然数の最大公約数を求める手法の一つである。

ユークリッドの互除法で再帰を用いて一般化すると次のように表せる。

	gcd(x,y) = { x                (y == 0 のとき)
	           { gcd(y, x mod y)  (y != 0 のとき)

ユークリッドの互除法で再帰を用いて最大公約数を求める関数を作成せよ。

さらに、動作を確認するためにmain関数にも処理を加えよ。処理例は以下に示す。

Console:

	input 2 numbers > 123 456
	gcd is 3

参照）gcd は greatest common divisor（最大公約数）の略である。

[Answer](#func3)

## 課題4. 文字列の圧縮

文字列「aaabbcaaaa」が与えられた時、文字列「abca」を返すように、同じ文字を圧縮する関数を作成せよ。

さらに、動作を確認するためにmain関数にも処理を加えよ。処理例は以下に示す。

Console:

	input a string > hheeeeeellooooo!!
	result is "helo!"

[Answer](#func4)

## 課題5. カンマ区切り

long int型を用いて桁の多い数値を引数とし、3桁ずつカンマで区切って出力する関数を作成せよ。

Console:

	input a number (under 9223372036854775807)
	> 12345678900000
	12,345,678,900,000

[Answer](#func5)






----

# 関数(Function) - 回答

<a name="func1"></a>
## 1. 素数判定器
~~~ c
#include <stdio.h>
#include <string.h>

int primality_test(int num){
	int i;
	int isPrime = 1;
	if (num <= 1) return 0;
	for (i = 2; i * i <= num; i++) {
		if (num % i == 0) {
			isPrime = 0;
			break;
		}
	}
	return isPrime;
}

int main(void){
	int num;
	char result[10];
	printf("input a number > "); scanf("%d", &num);
	strcpy(result, primality_test(num) ? "prime" : "not prime");
	printf("%d is %s\n", num, result);
}
~~~

<a name="func2"></a>
## 2. 曜日
~~~ c
#include <stdio.h>

void day_of_week(int y, int m, int d){
	int d_w = (y + y/4 - y/100 + y/400 + (13 * m + 8)/5 + d) % 7;
	switch (d_w) {
		case 0: puts("Sun"); break;
		case 1: puts("Mon"); break;
		case 2: puts("Tue"); break;
		case 3: puts("Wed"); break;
		case 4: puts("Thu"); break;
		case 5: puts("Fri"); break;
		case 6: puts("Sat"); break;
	}
}

int main(void){
	int year, month, day;
	printf("input year  > "); scanf("%d", &year);
	printf("input month > "); scanf("%d", &month);
	printf("input day   > "); scanf("%d", &day);
	day_of_week(year, month, day);
}
~~~

<a name="func3"></a>
## 3. ユークリッドの互除法
~~~ c
#include <stdio.h>

int gcd(int x, int y){
	return y == 0 ? x : gcd(y, x % y);
}

int main(void){
	int num1, num2;
	printf("input 2 numbers > "); scanf("%d %d", &num1, &num2);
	printf("gcd is %d\n", gcd(num1, num2));
}
~~~

<a name="func4"></a>
## 4. 文字列の圧縮
~~~ c
#include <stdio.h>

char* squeeze(char str[], char squeezed[]){
	int from_i, to_i;
	char before = 0;

	for (from_i = 0; str[from_i] != '\0'; from_i++) {
		if (str[from_i] != before) {
			squeezed[to_i++] = before = str[from_i];
		}
	}
	return squeezed;
}

int main(void){
	char str[100];
	char squeezed[100];
	printf("input a string > "); scanf("%s", str);
	squeeze(str, squeezed);
	printf("result is \"%s\"\n", squeezed);
}
~~~

<a name="func5"></a>
## 5. カンマ区切り
~~~ c
#include <stdio.h>
#include <limits.h>

void separate_with_comma(long int num){
	int i, digit_num;
	const int max_digit = 20;
	int digit[max_digit];

	// 各桁の数字を配列の要素として格納する
	for (i = 0; i < max_digit; i++) {
		digit[i] = num % 10;
		num /= 10;
		if (num <= 0) {
			digit_num = i; // 桁数を記録
			break;
		}
	}
	
	// 3桁ずつカンマで区切って出力
	for (i = digit_num; i >= 0; i--) {
		printf("%d", digit[i]);
		if (i % 3 == 0 && i != 0) {
			putchar(',');
		}
	}
	putchar('\n');
}

int main(void){
	unsigned long int num;
	printf("input a number (under %lu)\n", LONG_MAX);
	printf("> "); scanf("%ld", &num);
	separate_with_comma(num);
}
~~~











