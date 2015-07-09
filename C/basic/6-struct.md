
C言語プログラミング問題集 基礎
==========================

プログラミングの問題をまとめました。全文転載はご遠慮ください。問題点はissuesで受け付けています。

# 構造体(Structure)

- 先修 : 関数(Function)
- 後修 : ポインタ

はじめに、構造体のおさらい

~~~ c
// Defining a Structure
struct tag {
	type member1;
	type member2;
	...
};

int main(){
	// Declaring Structure
	struct tag Name;
	// Accessing Structure Members
	Name.member1 = 16;
}
~~~


## 課題1. 成績

次のような成績表を構造体の配列を用いて作成せよ。

| 名前 | 得点
|-----------|------
| sato      | 78
| suzuki    | 86
| takahashi | 92
| tanaka    | 74
| ito       | 81
| watanabe  | 85

次に、作った構造体にアクセスして、6人の得点の平均点も求めて出力せよ。

Console:

	average = 82.666667

[Answer](#struct1)

## 課題2. 座標

x,y座標をメンバとする構造体を作成せよ。

~~~ c
typedef struct {
	double x;
	double y;
} Point;
~~~

次に、2つの構造体を引数として、それらの距離を返す__関数__を作成せよ。ただし、2点の距離を求めるには次の式を用いること。
	
	2点 (a,b), (c,d) 間の距離は次の式で与えられる。 

	d = √{(a - c)^2 + (b - d)^2}

次の出力例は2点(3,5), (-2,8)で計算させた結果である。

Console example:
	
	the distance of two points is 6.708204

[Answer](#struct2)

## 課題3. 時間

トラックを4周する競技で、1周ごとにランナーのタイムを記録するプログラムを作成する。具体的には、各ラップをキーボードから入力し、合計タイムを出力する。

Console:

	lap 1 (hh:mm:ss) > 1:00:10
	lap 2 (hh:mm:ss) > 1:10:20
	lap 3 (hh:mm:ss) > 1:20:30
	lap 4 (hh:mm:ss) > 1:30:40
	total time = 05:01:40

[Answer](#struct3)




----

# 構造体(Structure) - 回答

<a name="struct1"></a>
## 1. 成績
~~~ c
#include <stdio.h>
#include <limits.h>

typedef struct {
	char name[255];
	int score;
} Student;

int main(void){
	int i, sum;
	const int size = 6;
	Student students[size] = {
		"sato", 	78, 
		"suzuki", 	86, 
		"takahashi",92, 
		"tanaka", 	74, 
		"ito", 		81, 
		"watanabe", 85
	};

	sum = 0;
	for (i = 0; i < size; i++) {
		sum += students[i].score;
	}
	printf("average = %f\n", (double)sum / size);
}
~~~

<a name="struct2"></a>
## 2. 座標
~~~ c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Point;

double distance(Point p1, Point p2){
	double x = pow(p1.x - p2.x, 2);
	double y = pow(p1.y - p2.y, 2);
	double dist = sqrt(x + y);
	return dist;
}

int main(void){
	Point p1 = { 3, 5};
	Point p2 = {-3, 8};
	double dist = distance(p1, p2);
	printf("the distance of two points is %f\n", dist);
}
~~~

<a name="struct3"></a>
## 3. 時間
~~~ c

#include <stdio.h>
#include <time.h>

typedef struct {
	int hour;
	int min;
	int sec;
} Time;

Time set_time(int count){
	Time now = {0, 0, 0};
	printf("lap %d (hh:mm:ss) > ", count + 1);
	scanf("%d:%d:%d", &now.hour, &now.min, &now.sec);
	return now;
}

Time add_time(Time tm1, Time tm2){
	tm1.hour += tm2.hour;
	tm1.min += tm2.min;
	tm1.sec += tm2.sec;
	while (tm1.sec >= 60) {
		tm1.min += 1;
		tm1.sec -= 60;
	}
	while (tm1.min >= 60) {
		tm1.hour += 1;
		tm1.min -= 60;
	}
	return tm1;
}

int main(void){
	int i;
	const int MAX_LAP = 4;
	Time lap[MAX_LAP];
	Time total = {0, 0, 0};

	for (i = 0; i < MAX_LAP; i++) {
		lap[i] = set_time(i);
	}

	for (i = 0; i < MAX_LAP; i++) {
		total = add_time(total, lap[i]);
	}

	printf(
		"total time = %02d:%02d:%02d\n", 
		total.hour, 
		total.min, 
		total.sec
	);
}
~~~



















