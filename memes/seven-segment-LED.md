# 7セグメントLED
## 使用方法
1. 普通のLEDと同じように、まずは__PEIORL__レジスタでLEDの入出力設定を行う。ここでは1を代入し、出力モードにする。
2. 次に、__PAIORH__レジスタも初期化する必要があるため、0x0FをORで代入する。
3. LEDを点灯させるには、まず`PA.DR.BYTE.HL &= 0xF0`をしたのちに表示させたいデータをORで代入する。
4. 最後に、`PE.DR.BIT.B3 = 1`を行うと、LEDが点灯する。

MEMEsには3つの7セグメントLEDが実装されている。アクセスするには、右のLEDから順に`PE.DR.BIT.B3`, `PE.DR.BIT.B2`, `PE.DR.BIT.B1`である。

~~~ c
//
// DIG1 : PE3
//
// 7セグメントLEDの初期化
PFC.PEIORL.BIT.B3 = 1;
PFC.PAIORH.BYTE.L |= 0x0F;
// 7セグメントLEDの点灯
PA.DR.BYTE.HL &= 0xF0;
PA.DR.BYTE.HL |= num;
PE.DR.BIT.B3 = 1;
~~~

## サンプル

~~~ c
#include "iodefine.h"

#define     DIG1    (PE.DR.BIT.B3)

void main(void) {
    PFC.PEIORL.BIT.B3 = 1;
    PFC.PAIORH.BYTE.L |= 0x0F;

    while (1) {
        PA.DR.BYTE.HL &= 0xF0;
        PA.DR.BYTE.HL |= 6;
        DIG1 = 1;
    }
}
~~~
