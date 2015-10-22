
課題
----

日付を保存するためのクラス Date を作成せよ。

##### フィールド

- 西暦
- 月
- 日

##### メソッド

- 西暦を返すメソッド
- 月を返すメソッド
- 日にちを返すメソッド
- 出力する際、文字列に変換するメソッドのオーバーライド・オーバーロード
	- C++  : operator <<
	- Java : toString
	- Ruby : to_s

動作例
------

main.cpp

``` cpp
#include <iostream>
#include "date.h"

using namespace std;

int main(void) {
	Date date(2015, 4, 1);
	cout << "today is " << date << endl;
}
```

```
today is 2015/4/1
```

|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  


回答
-----

date.h

~~~ cpp
#include <iostream>

class Date {
private:
	int year, month, day;

public:
	Date(int year, int month, int day);

	int get_year()  const { return year; }
	int get_month() const { return month; }
	int get_day()   const { return day; }
};

Date::Date(int year, int month, int day) {
	this->year  = year;
	this->month = month;
	this->day   = day;
}

std::ostream& operator << (std::ostream &out, const Date date) {
	return out
		<< date.get_year() << "/" 
		<< date.get_month() << "/"
		<< date.get_day();
}
~~~













