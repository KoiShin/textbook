
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# 出力と入力

- 先修 : main関数, io(printf, scanf), 変数(int, float), 演算子(`+ - * / %`)
- 後修 : if文

はじめに、C言語のおまじないは長ったらしいので、下に貼っておきます。

Code:

~~~ c
#include <stdio.h>

int main(void){
	// ここでprintf関数を使おう
	return 0;
}
~~~

## 課題1. 改行の出力

下に示すような表示を行うプログラムを作成せよ。
ただし、プログラム内でのprintf関数の呼び出しは1回限りとする。

Output:

	hello
	c
	lang
	beginners

[Answer](#io1)

## 課題2. 数式の計算

次の数式をプログラムで計算して、答えを出力せよ。

	{ 24 ÷ 3 + (70 - 14) } × 16

Output:

	1024

[Answer](#io2)

## 課題3. 小数の出力

int型の変数a,bにそれぞれ10と3を代入する。
変数a,bの商を求めて小数第3位まで出力せよ。

Output:

	3.333

[Answer](#io3)

## 課題4. 出力形式

整数123を出力する際に以下のオプションを付加して、それぞれで出力せよ。

- 8桁まで表示して、表示できない桁は空白で埋まる
- 8桁まで表示して、表示できない桁は0で埋める
- 8桁まで表示して、左寄せにする

Output:

	     123
	00000123
	123

[Answer](#io4)

## 課題5. 値の読み取り

以下に示すように、読み込んだ整数を10倍した値を表示するプログラムを作成せよ。

Console:

	input a number > 57
	If multiply the number by 10, the result will be 570.

[Answer](#io5)

## 課題6. 時間の表示

以下に示すように、読み込んだ3つの整数を時,分,秒として出力するプログラムを作成せよ。

Console:

	input a time(hh mm ss) > 5 35 7
	inputted time is 05:35:07
	

[Answer](#io6)






----

# 出力と入力 - 回答

現在のレベルでは返り値を気にする必要はないため、`return 0;`は省略しています。

<a name="io1"></a>
## 1. 改行の出力
~~~ c
#include <stdio.h>

int main(void){
	printf("hello\nc\nlang\nbegginners\n");
}
~~~

<a name="io2"></a>
## 2. 数式の計算
~~~ c
#include <stdio.h>

int main(void){
	int ans = (24 / 3 + (70 - 14)) * 16;
	printf("%d\n", ans);
}
~~~

<a name="io3"></a>
## 3. 小数の出力
~~~ c
#include <stdio.h>

int main(void){
	float ans = 10.0 / 3.0;
	printf("%.3f\n", ans);
}
~~~

<a name="io4"></a>
## 4. 出力形式
~~~ c
#include <stdio.h>

int main(void){
	int a = 123;
	printf("%8d\n", a);
	printf("%08d\n", a);
	printf("%-8d\n", a);
}
~~~

<a name="io5"></a>
## 5. 値の読み取り
~~~ c
#include <stdio.h>

int main(void){
	int input;
	printf("input a number > "); scanf("%d", &input);

	printf(
		"If multiply the number by 10, the result will be %d\n", 
		input * 10
	);
}
~~~

<a name="io6"></a>
## 6. 時間の表示
~~~ c
#include <stdio.h>

int main(void){
	int hour, min, sec;
	printf("input a time(hh mm ss) > "); scanf("%d %d %d", &hour, &min, &sec);

	printf(
		"inputted time is %02d:%02d:%02d\n",
		hour, min, sec
	);
}
~~~











