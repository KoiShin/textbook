
LED
===========

MEMEsにはLEDが2つあり、右からLED5、LED6が配置されている。

使用方法
-----------

### 手順

1. LEDを点灯させるには、まず__PEIORL__レジスタでLEDの入出力設定を行う。
0 は入力、1 は出力を意味する。
LEDは出力しか行わないので、PEIORLレジスタには1を代入する。

2. 次に、__PEDRL__レジスタに0（または1）を代入することで、LEDの点灯（または消灯）を行うことができる。

~~~ c
// 
// LED5 : PE9
// LED6 : PE11
// 
// LEDの入出力設定
PFC.PEIORL.BIT.B9  = 1;
PFC.PEIORL.BIT.B11 = 1;
// LEDの点灯消灯（0: 点灯、1: 消灯）
PE.DR.BIT.B9 = 0;
PE.DR.BIT.B11 = 0;
~~~

### 同時点灯

LED5とLED6を同時（数μsのズレなし）に点灯させるには、__BYTE__や__WORD__や__LONG__で特定のビットを操作する必要がある。
点灯させるために操作するビットは、PEDRLレジスタのビット9とビット11を0にする必要がある。
そこで論理積の演算子 `&` を用いる。C言語では、`a = a & b` と `a &= b` は同等である。

~~~ c
// 2つのLEDの同時点灯
PE.DR.BYTE.LH &= 0xF5;
PE.DR.WORD.L &= 0xF5FF;
PE.DR.LONG &= 0xFFFFF5FF;
~~~

`PE.DR.WORD.L`を例に、`PE.DR.WORD.L &= 0xF5FF`が行っていることを図に起こすと次のような感じである。

         PE.DR.WORD.L   **** **** **** ****
    and        0xF5FF   1111 0101 1111 1111
    ---------------------------------------
         PE.DR.WORD.L   **** 0*0* **** ****

右側からビット0、ビット1 ... ビット15 と並んでおり、論理積`&`によって`PE.DR.WORD.L`のビット9とビット11だけ0にすることができた。

### 同時消灯

また2つのLEDを同時に消灯させるには、PEDRLレジスタのビット9とビット11を1にする必要があるので、以下のコードを書く。

~~~ c
// 2つのLEDの同時消灯
PE.DR.BYTE.LH |= 0x0A;
PE.DR.WORD.L |= 0x0A00;
PE.DR.LONG |= 0x00000A00;
~~~

`PE.DR.WORD.L`を例に、`PE.DR.WORD.L |= 0x0A00`が行っていることを図に起こすと次のような感じである。

         PE.DR.WORD.L   **** **** **** ****
    or         0x0A00   0000 1010 0000 0000
    ---------------------------------------
         PE.DR.WORD.L   **** 1*1* **** ****

論理和`|`によって`PE.DR.WORD.L`のビット9とビット11だけ1にすることができる。

サンプル
-----------

~~~ c
#include "iodefine.h"

void main(void) {
	// set the LED IO
	PFC.PEIORL.BIT.B9 = 1;
	PFC.PEIORL.BIT.B11 = 1;
	
	// LED on
	PE.DR.BIT.B9 = 0;
	PE.DR.BIT.B11 = 0;

	// LED off
	PE.DR.BIT.B9 = 1;
	PE.DR.BIT.B11 = 1;
	
	while(1)
		;
}
~~~

