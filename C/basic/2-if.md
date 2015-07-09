
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# if文

- 先修 : io(printf, scanf), 比較演算子, 条件演算子(`?:`), 論理演算子
- 後修 : for文

はじめに、if文のおさらい

~~~ c
if (statement1) {
	// Execute these
	// if statement1 is TRUE
} else if (statement2) {
	// Execute these
	// if statement1 is FALSE and statement2 is TRUE
} else {
	// Execute these
	// if all statements is FALSE
}
~~~

## 課題1. 値の比較

3つの整数を読み込んで、その最大値を表示せよ。

Console:

	input 3 numbers > 23 42 9
	max value is 42

[Answer](#if1)

## 課題2. 倍数判定

整数を読み込んで、2の倍数なら「_num_ is multiple of 2」と出力せよ。
2の倍数でない場合は「_num_ is not multiple of 2」と出力せよ。

Console:

	input a number > 1024
	1024 is multiple of 2

	input a number > 1023
	1023 is not multiple of 2

[Answer](#if2)

## 課題3. 絶対値

整数を読み込んで、その絶対値を出力せよ。

Console:

	input a number > -8
	|-8| == 8

	input a number > 4
	|4| == 4

[Answer](#if3)

## 課題4. うるう年の判定

うるう年は以下のように定義されている。(wikipediaより引用)

> グレゴリオ暦では、次の規則に従って400年間に（100回ではなく）97回の閏年を設ける。
>
> 1. 西暦年が4で割り切れる年は閏年
> 2. ただし、西暦年が100で割り切れる年は平年
> 3. ただし、西暦年が400で割り切れる年は閏年

以上の規則をもとに、入力された年がうるう年かどうかを判定するプログラムを作成せよ。

Console:

	input a year > 2000
	leap year

	input a year > 2100
	not leap year
	
	input a year > 2104
	leap year

[Answer](#if4)



----

# if文 - 回答

<a name="if1"></a>
## 1. 値の比較
~~~ c
#include <stdio.h>

int main(void){
	int max;
	int num1, num2, num3;
	printf("input 3 numbers > "); scanf("%d %d %d", &num1, &num2, &num3);

	max = num1;
	if (num2 > max) max = num2;
	if (num3 > max) max = num3;

	printf("max value is %d\n", max);
}
~~~

<a name="if2"></a>
## 2. 倍数判定
~~~ c
#include <stdio.h>

int main(void){
	int num;
	printf("input a number > "); scanf("%d", &num);

	if (num % 2 == 0) {
		printf("%d is multiple of 2\n", num);
	} else {
		printf("%d is not multiple of 2\n", num);
	}
}
~~~

<a name="if3"></a>
## 3. 絶対値
~~~ c
#include <stdio.h>

int main(void){
	int num, abs;
	printf("input a number > "); scanf("%d", &num);

	abs = num < 0 ? -num : num;
	printf("|%d| == %d\n", num, abs);
}
~~~

<a name="if4"></a>
## 4. うるう年の判定
◎ Good:

~~~ c
#include <stdio.h>

int main(void){
	int year;
	printf("input a year > "); scanf("%d", &year);

	if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
		printf("leap year\n");
	} else {
		printf("not leap year\n");
	}
}
~~~

◯ Yes:

~~~ c
#include <stdio.h>

int main(void){
	int year;
	printf("input a year > "); scanf("%d", &year);

	if (year % 400 == 0) {
		printf("leap year\n");
	} else if (year % 100 == 0) {
		printf("not leap year\n");
	} else if (year % 4 == 0) {
		printf("leap year\n");
	} else {
		printf("not leap year\n");
	}
}
~~~

× Bad:

~~~ c
#include <stdio.h>

int main(void){
	int year;
	printf("input a year > "); scanf("%d", &year);

	if (year % 400 == 0) {
		printf("leap year\n");
	} else {
		if (year % 100 == 0) {
			printf("not leap year\n");
		} else if (year % 4 == 0) {
			printf("leap year\n");
		} else {
			printf("not leap year\n");
		}
	}
}
~~~




----

## おまけ（時間を持て余している方へ）

if文にはいろんな書き方がありますが、以下に示す入れ子になったif文について質問です。

~~~ c
if (state1) if (state2) something(); else something_else();
~~~

この入れ子のif文は2つの解釈ができます。

~~~ c
// 解釈1
if (state1) {
	if (state2) {
		something();
	}
} else {
	something_else();
}
~~~

~~~ c
// 解釈2
if (state1) {
	if (state2) {
		something();
	} else {
		something_else();
	}
}
~~~

つまり、elseは1つ目のif文に所属するのか、2つ目のif文に所属するのか、ということです。
手元でネストした1行if文と同じ状況を作り出し、Cコンパイラの解釈の挙動を確認せよ。

(注) Cコンパイラが「elseの位置が紛らわしいから`{}`付けろ!」と警告を出しますが、実行可能です。














