
Switch
==========

MEMEsにはスイッチが3つあり、右からSW4、SW5、SW6が配置されている。

使用方法
-----------

### 手順

1. スイッチSW4〜SW6を入力として使用するには、__PDIORH__レジスタのビット16〜18を0（入力設定）にする。

2. スイッチSW4〜SW6が押されているか確認するには、__PDDRL__レジスタのビット16〜18が1なら押されている状態である。

~~~ c
// 
// SW4 : PD16
// SW5 : PD17
// SW6 : PD18
// 
// スイッチの入出力設定（0: 入力）
PFC.PDIORH.BIT.B16 = 0;
PFC.PDIORH.BIT.B17 = 0;
PFC.PDIORH.BIT.B18 = 0;
// スイッチが押されているかの確認方法
if (PD.DR.BIT.B16 == 1) { /* .. */ }
if (PD.DR.BIT.B17 == 1) { /* .. */ }
if (PD.DR.BIT.B18 == 1) { /* .. */ }
~~~

### スイッチを離した時

スイッチを押してから、離した時に何かを実行させる場合は、次のようにコードを書きます。

~~~ c
if (PD.DR.BIT.B16) {
	while (PD.DR.BIT.B16)
		;
	do_something(); // スイッチを離した時に実行される
}
~~~


サンプル
----------

以下は、

- スイッチSW5を押すとLED5の状態が変わる（onからoff、またはoffからon）
- スイッチSW6を押すとLED6の状態が変わる
- スイッチSW4を押すと2つのLEDの状態が変わる

プログラムです。

~~~c
#include "iodefine.h"
#define  LED5  (PE.DR.BIT.B9)
#define  LED6  (PE.DR.BIT.B11)
#define  LED_ON  (0)
#define  LED_OFF (1)
#define  SW4   (PD.DR.BIT.B16)
#define  SW5   (PD.DR.BIT.B17)
#define  SW6   (PD.DR.BIT.B18)

void main(void) {
	// set the LED IO
	PFC.PEIORL.BIT.B9  = 1;
	PFC.PEIORL.BIT.B11 = 1;
	// set the Switch IO
	PFC.PDIORH.BIT.B16 = 0;
	PFC.PDIORH.BIT.B17 = 0;
	PFC.PDIORH.BIT.B18 = 0;

	LED5 = LED6 = LED_OFF;

	while (1) {
		if (SW4) {
			while (SW4)
				;
			// invert LED5 and LED6
			PE.DR.LONG ^= 0x0A00;
		}

		if (SW5) {
			while (SW5)
				;
			// turn LED5
			LED5 = (LED5 == LED_ON) ? LED_OFF : LED_ON;
		}
		
		if (SW6) {
			while (SW6)
				;
			// turn LED6
			LED6 = (LED6 == LED_ON) ? LED_OFF : LED_ON;
		}
	}
}
~~~
