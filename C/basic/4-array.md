
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# 配列(Array)

- 先修 : for文, 乱数(rand), 文字列(String)
- 後修 : 関数

はじめに、配列のおさらい

~~~ c
// Declaring arrays
int array1[100];
// Initializing arrays
int array2[] = {3,1,4};
// Accessing array elements
int num = array2[0];
array2[0] = 10;

// Declaring two-dimensional arrays
int array3[10][10];
// Initializing two-dimensional arrays
int array4[][] = {
	{1,2,3},
	{4,5,6},
	{7,8,9}
}
// Accessing array elements
int num = array4[0][1];
array4[0][1] = 10;
~~~


## 課題1. 配列の全要素の出力

配列の全要素を出力するプログラムを作成せよ。
出力する配列は以下に示す。

~~~ c
const int size = 10;
int ary[size] = {3,1,4,1,5,9,2};  // 出力する配列ary
~~~

Output Example:

	array[0] = 3
	array[1] = 1
	array[2] = 4
	array[3] = 1
	array[4] = 5
	array[5] = 9
	array[6] = 2
	array[7] = 0
	array[8] = 0
	array[9] = 0

[Answer](#array1)

## 課題2. フィボナッチ数列

n番目のフィボナッチ数を`F_n`で表すと、フィボナッチ数列は次のように定義される。

	F_0 = 0
	F_1 = 1
	F_{n+2} = F_n + F_{n+1}  (n >= 0)

フィボナッチ数列の第30項までを__for文を使って__配列に格納し、その配列の全要素を出力せよ。

~~~ c
// 数列を格納する配列（イメージ）
sequence[30] = {0, 1, 1, 2, 3, 5, 8, ...}
~~~

Output:

	0
	1
	1
	2
	3
	5
	8
	13
	21
	34
	55
	89
	144
	...(省略)...
	317811
	514229
	
[Answer](#array2)

## 課題3. 辞書

はじめに20文字以内の単語を100個格納できる配列dictionaryを宣言する。
また、デフォルトで文字列「apple, bread, candy」を格納する。

	char dictionary[100][20] = {"apple", "bread", "candy"}

この辞書に対して、検索と追加を行うプログラムを作成せよ。
具体的な例は以下に示す。

Console:

	input commands and words
	> find candy
	found: candy
	> push year
	pushed to dictionary: year
	> push day
	pushed to dictionary: day
	> find year
	found: year
	> find minute
	not found
	> ^C

[Answer](#array3)

## 課題4. 統計

プログラミング試験の30人分の結果を格納する配列resultsがある。

	int results[30] = {
		67,81,47,74,86,72,41,26,80,89,
		78,63,73,56,90,73,67,92,93,77,
		79,85,68,83,96,58,88,69,70,98
	}

この試験結果について以下の項目をそれぞれ求めて出力せよ。

- 最高点
- 最低点
- [平均（相加平均）](https://ja.wikipedia.org/wiki/%E5%B9%B3%E5%9D%87)
- [分散（母分散）](https://ja.wikipedia.org/wiki/%E5%88%86%E6%95%A3)
- [標準偏差](https://ja.wikipedia.org/wiki/%E6%A8%99%E6%BA%96%E5%81%8F%E5%B7%AE)

計算式はそれぞれのリンクを参照すること。
分散や標準偏差が全くわからない人も平均を求めるところまでは必ずやりましょう。

Output:

	max = 98
	min = 26
	average = 73.966667
	variance = 263.565556
	std_deviation = 16.234702
	
[Answer](#array4)

## 課題5. マインスイーパ(Minesweeper)

ここではマインスイーパというゲームを作るのではなく、このゲームの基本的な部分を作ることにする。

作成するプログラムの概要を以下にまとめる。

1. 10 × 10 のフィールド`mines[x][y]`を用意する
2. 各マスに対して、ランダムに地雷を埋める
3. minesの内容を出力する
4. 各マスの周りの地雷の数を数えて、その結果を`view[x][y]`に格納する
5. fieldの内容を出力する

出力結果は以下のようにする。

Output Example:

	mines field
	0 0 0 0 0 0 0 0 0 0 
	0 0 0 0 0 0 0 0 0 0 
	0 0 0 0 1 0 0 0 0 0 
	0 0 1 0 0 0 0 0 0 0 
	0 0 0 1 0 0 0 1 0 1 
	1 0 0 0 0 0 0 0 0 0 
	0 0 0 0 0 0 0 1 0 0 
	0 0 1 0 1 1 0 0 0 0 
	0 0 0 0 1 0 1 0 0 0 
	0 0 0 1 0 0 0 0 0 0 

	view field
	0 0 0 0 0 0 0 0 0 0 
	0 0 0 1 1 1 0 0 0 0 
	0 1 1 2 * 1 0 0 0 0 
	0 1 * 3 2 1 1 1 2 1 
	1 2 2 * 1 0 1 * 2 * 
	* 1 1 1 1 0 2 2 3 1 
	1 2 1 2 2 2 2 * 1 0 
	0 1 * 3 * * 3 2 1 0 
	0 1 2 4 * 4 * 1 0 0 
	0 0 1 * 2 2 1 1 0 0 

mines fieldについて、`1`は地雷がある状態、`0`は何もない状態を示す。

view fieldについて、`*`は地雷の場所、`n(数字)`は周りにある地雷の数を示す。

[Answer](#array5)






----

# 配列(Array) - 回答

<a name="array1"></a>
## 1. 配列の全要素の出力
~~~ c
#include <stdio.h>
 
int main(void){
	int i;
	const int size = 10;
	int array[size] = {3,1,4,1,5,9,2};
	for (i = 0; i < size; i++) {
		printf("array[%d] = %d\n", i, array[i]);
	}
}
~~~

<a name="array2"></a>
## 2. フィボナッチ数列
~~~ c
#include <stdio.h>
 
int main(void){
	int i;
	const int size = 30;
	int fibo[size];
	
	fibo[0] = 0;
	fibo[1] = 1;
	for (i = 2; i < size; i++) {
		fibo[i] = fibo[i - 1] + fibo[i - 2];
	}

	for (i = 0; i < size; i++) {
		printf("%d\n", fibo[i]);
	}
}
~~~

<a name="array3"></a>
## 3. 辞書
~~~ c
#include <stdio.h>
#include <string.h>

int main(void){
	int i;
	const int size = 100;
	const int word_len = 20;
	char dictionary[size][word_len] = {"apple", "bread", "candy"};
	int stack_point = 3;
	char command[word_len];
	char word[word_len];

	puts("input commands and words");
	for (printf("> "); scanf("%s %s", command, word); printf("> ")) {
		// find mode
		if (strcmp(command, "find") == 0) {
			int isFound = 0;
			for (i = 0; i < size; i++) {
				if (strcmp(dictionary[i], word) == 0) {
					isFound = 1;
					break;
				}
			}
			if (isFound) {
				printf("found: %s\n", dictionary[i]);
			} else {
				printf("not found\n");
			}
			continue;
		}
		// push mode
		if (strcmp(command, "push") == 0) {
			if (stack_point > size) {
				puts("dictionary is full");
				continue;
			}
			strcpy(dictionary[stack_point], word);
			stack_point++;
			printf("pushed to dictionary: %s\n", word);
			continue;
		}

		// else
		puts("usage: [find | push] <word>");
	}
}
~~~

<a name="array4"></a>
## 4. 統計
~~~ c

#include <stdio.h>
#include <math.h>
 
int main(void){
	int i, sum;
	double d_sum;
	const int size = 30;
	int results[size] = {
		67,81,47,74,86,72,41,26,80,89,
		78,63,73,56,90,73,67,92,93,77,
		79,85,68,83,96,58,88,69,70,98
	};

	int max, min;  // 最大値、最小値
	double average;    // 平均値
	double deviations[size]; // 偏差
	double variance;         // 分散
	double std_deviation;    // 標準偏差

	// find max and min
	max = min = results[0];
	for (i = 1; i < size; i++) {
		if (results[i] > max) {
			max = results[i];
		} else if (results[i] < min) {
			min = results[i];
		}
	}
	printf("max = %d\n", max);
	printf("min = %d\n", min);

	// find average value
	sum = 0;
	for (i = 0; i < size; i++) {
		sum += results[i];
	}
	average = (double)sum / size;
	printf("average = %f\n", average);

	// find variance
	for (i = 0; i < size; i++) {
		deviations[i] = results[i] - average;
	}
	d_sum = 0;
	for (i = 0; i < size; i++) {
		d_sum += deviations[i] * deviations[i];
	}
	variance = d_sum / size;
	printf("variance = %f\n", variance);

	// find standard deviation
	std_deviation = sqrt(variance);
	printf("std_deviation = %f\n", std_deviation);
}
~~~

<a name="array5"></a>
## 5. マインスイーパ(Minesweeper)
~~~ c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void){
	int i, j;
	const int size = 12;
	int mines[size][size];
	char view[size][size];

	// init mines
	srand(time(NULL));
	for (i = 1; i < size - 1; i++) {
		for (j = 1; j < size - 1; j++) {
			// 1/5の確率で地雷を埋める
			mines[i][j] = rand() % 100 < 20 ? 1 : 0;
		}
	}

	// output mines
	puts("mines field");
	for (i = 1; i < size - 1; i++) {
		for (j = 1; j < size -1; j++) {
			printf("%d ", mines[i][j]);
		}
		putchar('\n');
	}
	putchar('\n');

	// 各座標のマスの見た目を決定する
	for (i = 1; i < size - 1; i++) {
		for (j = 1; j < size - 1; j++) {
			int r_i, r_j;
			int count = 0;

			// mines[i][j]に地雷があれば、viewに'*'を格納する
			if (mines[i][j]) {
				view[i][j] = '*';
				continue;
			}

			// 周りの地雷の数を数える
			for (r_i = -1; r_i <= 1; r_i++) {
				for (r_j = -1; r_j <= 1; r_j++) {
					if (mines[i + r_i][j + r_j] == 1) count++; 
				}
			}
			// 数えた数を文字としてviewに格納
			view[i][j] = '0' + count;
		}
	}

	// output view
	puts("view field");
	for (i = 1; i < size - 1; i++) {
		for (j = 1; j < size - 1; j++) {
			printf("%c ", view[i][j]);
		}
		putchar('\n');
	}
}
~~~
