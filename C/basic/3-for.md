
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# for文

- 先修 : if文, 演算子(`++ --`)
- 後修 : 配列

はじめに、for文のおさらい

~~~ c
for (var init; condition; update) {
	// Code to execute while the condition is true
}
~~~

## 課題1. FizzBuzz

FizzBuzzとは以下のように定義されている（wikipediaより引用、一部省略）。

> #### 遊び方
> 
> 最初のプレイヤーは「1」と数字を発言する。
> 次のプレイヤーは直前のプレイヤーの次の数字を発言していく。
> ただし、3で割り切れる場合は 「Fizz」、
> 5で割り切れる場合は 「Buzz」、
> 両者で割り切れる場合は 「FizzBuzz」を数の代わりに発言しなければならない。

この遊びを20まで行い、for文を使って以下の画面のように出力するプログラムを作成せよ。

Output:

	1 2 Fizz 4 Buzz Fizz 7 8 Fizz Buzz 11 Fizz 13 14 FizzBuzz 16 17 Fizz 19 Buzz 

[Answer](#for1)

## 課題2. 階乗

読み込んだ整数の階乗をfor文で計算せよ。

ただし、nの階乗は次のように定義される。

	n! == 1 × 2 × 3 × ... × (n-1) × n

Console:

	input a number > 5
	5! == 120

[Answer](#for2)

## 課題3. 九九の表

多重ループを用いて、九九の表を出力せよ。

Output:

	 1  2  3  4  5  6  7  8  9 
	 2  4  6  8 10 12 14 16 18 
	 3  6  9 12 15 18 21 24 27 
	 4  8 12 16 20 24 28 32 36 
	 5 10 15 20 25 30 35 40 45 
	 6 12 18 24 30 36 42 48 54 
	 7 14 21 28 35 42 49 56 63 
	 8 16 24 32 40 48 56 64 72 
	 9 18 27 36 45 54 63 72 81 

[Answer](#for3)

## 課題4. 円周率

一番簡単な（そして収束の遅い）以下の式で円周率を求めて出力せよ。

	π/4 == 1 - 1/3 + 1/5 - 1/7 + ...

右辺は100万項くらいを目安にするとよい。

Output:
	
	3.141591

[Answer](#for4)

## 課題5. 素数

読み込んだ整数が素数かどうかを判定するプログラムを作成せよ。

Console:

	input a number > 97
	97 is prime.

	input a number > 112
	112 is not prime.

[Answer](#for5)


----

# for文 - 回答

<a name="for1"></a>
## 1. FizzBuzz
~~~ c
#include <stdio.h>
 
int main(void){
    int i;
    for (i = 1; i <= 20; i++) {
        if (i % 3 == 0 && i % 5 == 0) {
            printf("FizzBuzz ");
        } else if (i % 3 == 0) {
            printf("Fizz ");
        } else if (i % 5 == 0) {
            printf("Buzz ");
        } else {
            printf("%d ", i);
        }
    }
    printf("\n");
}
~~~

<a name="for2"></a>
## 2. 階乗
~~~ c
#include <stdio.h>

int main(void){
	int i, num;
	int prod = 1;

	printf("input a number > "); scanf("%d", &num);

	for (i = 1; i <= num; i++) {
		prod *= i;
	}

	printf("%d! == %d\n", num, prod);
}
~~~

<a name="for3"></a>
## 3. 九九の表
~~~ c
#include <stdio.h>
 
int main(void){
    int i, j;
    for (i = 1; i < 10; i++) {
        for (j = 1; j < 10; j++) {
        	printf("%2d ", i * j);
        }
        printf("\n");
    }
}
~~~

<a name="for4"></a>
## 4. 円周率
~~~ c
#include <stdio.h>
 
int main(void){
	int i;
	int flag = 1;
	double sum = 0;
	for (i = 1; i < 1000000; i += 2) {
		sum += flag * 1.0 / i;
		flag = -flag;
	}
	sum *= 4;
	printf("%f\n", sum);
}
~~~

<a name="for5"></a>
## 5. 素数
~~~ c
#include <stdio.h>
 
int main(void){
	int i;
	int num;
	int isPrime = 1;
	printf("input a number > "); scanf("%d", &num);

	for (i = 2; i * i <= num; i++) {
		if (num % i == 0) {
			printf("%d is not prime.\n", num);
			isPrime = 0;
			break;
		}
	}

	if (isPrime) {
		printf("%d is prime.\n", num);
	}
}
~~~




----

## おまけ（時間を持て余している方へ）

与えられた数の平方根を求めるプログラムを作成する。

### 方法

正数`C`を平方根を求めるには、`x^2 - C = 0` を満たす`x`を求めれば良い。

`f(x) = x^2 - C` について、`x_0`における傾きは

	f'(x_0) = 2x_0 

で与えられる。接点がx軸と交わる点を`x_1`とすると、
傾きは `yの増加分 / xの増加分` であるから

	f'(x_0) = f(x_0) / (x_0 - x_1)

であり、`x_1`について解き、傾きの式を代入すれば

	x_1 = x_0 - f(x_0) / f'(x_0)
	    = x_0 - (x_0^2 - C) / 2x_0
	    = (x_0 + C / x_0) / 2

となる。求めた`x_1`を新たな出発点として、この手続を繰り返すことで、
x軸と接点との交点は、放物線とx軸との交点、すなわち、`√C` の値に近づく。

この一連の手続きを

	x_{n+1} = (x_n + C / x_n) / 2

と表すことができる。

----

math.h をincludeすれば、平方根を求める関数sqrt()が使えるが、今回はsqrt関数を使わずに、
ニュートン法（ニュートン - ラフソン法）を用いて与えられた数の平方根を求めよ。

Console:

	input a number > 2
	sqrt(2) = 1.414214

	input a number > 3
	sqrt(3) = 1.732051

[Answer](https://gist.github.com/TeX2e/68b8b967807d5c32989c)
















