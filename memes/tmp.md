
LED_ON_OFF
------------

~~~c
#include "iodefine.h"

#define printf ((int (*)(const char *,...))0x00007c7c)
#define scanf  ((int (*)(const char *,...))0x00007cb8)

void main(void) {
	// LED5
	// PFC.PEIORL.WORD |= 0x0200;
	// PE.DR.WORD.L &= 0xFDFF;
	
	PFC.PEIORL.BIT.B9 = 1;
	PFC.PEIORL.BIT.B11 = 1;
	
	printf("%x\n", PE.DR.BIT.B9);
	printf("%x\n", PE.DR.BIT.B11);
	
	PE.DR.BIT.B9 = 0;
	PE.DR.BIT.B11 = 0;
	// PE.DR.BYTE.LH &= 0xFD;
	// PE.DR.WORD.L &= 0xFDFF;
	// PE.DR.LONG &= 0xFFFFFDFF;
	
	printf("%x\n", PE.DR.BIT.B9);
	printf("%x\n", PE.DR.BIT.B11);
	
	while(1)
		;
}
~~~

LED_SW
------------

~~~c
#include "iodefine.h"
#define  LED5  (PE.DR.BIT.B9)
#define  LED6  (PE.DR.BIT.B11)
#define  SW4   (PD.DR.BIT.B16)
#define  SW5   (PD.DR.BIT.B17)
#define  SW6   (PD.DR.BIT.B18)
#define  LED_ON  (0)
#define  LED_OFF (1)
#define  SW_ON  (1)
#define  SW_OFF (0)
#define  LED_ON  (0)
#define  LED_OFF (1)

void main(void) {
	PFC.PEIORL.BIT.B9  = 1;
	PFC.PEIORL.BIT.B11 = 1;
	PFC.PDIORH.BIT.B16 = 0;
	PFC.PDIORH.BIT.B17 = 0;
	PFC.PDIORH.BIT.B18 = 0;
	LED5 = LED6 = LED_OFF;
	while (1) {
		if (SW5) {
			while (SW5);
			// turn LED5
			LED5 = (LED5 == LED_ON) ? LED_OFF : LED_ON;
		}
		
		if (SW6) {
			while (SW6);
			// turn LED6
			LED6 = (LED6 == LED_ON) ? LED_OFF : LED_ON;
		}

		if (SW4) {
			while (SW4);
			// invert LED5 and LED6
			// 0000101000000000
			// 1111010111111111
			PE.DR.LONG ^= 0x0A00;
		}
	}
}
~~~

~~~c
#include "iodefine.h"

void main(void) {
	PFC.PEIORL.WORD |= 0x0800;
	PFC.PDIORH.WORD &= 0xfffB;
	PE.DR.BIT.B11 = 1;
	while (1) {
		if(PD.DR.BIT.B18){
			while(PD.DR.BIT.B18);
			if (PE.DR.BIT.B11) {
				PE.DR.BIT.B11 = 0;
			} else {
				PE.DR.BIT.B11 = 1;
			}
		}
	}
}
~~~

CMT
------------

~~~c
#include "iodefine.h"

#define  LED5  (PE.DR.BIT.B9)
#define  LED6  (PE.DR.BIT.B11)
#define  LED_ON  (0)
#define  LED_OFF (1)

void main(void)
{
	STB.CR4.BIT._CMT = 0;		// モジュールスタンバイ解除

	PFC.PEIORL.BIT.B9 = 1;		// PE9(LED5) .. 出力
	PFC.PEIORL.BIT.B11 = 1;		// PE11(LED5) .. 出力
	LED5 = LED_OFF;
	LED6 = LED_ON;

	//CMT0.CMCSR.BIT.CKS = 1;		// ① CKS設定(0:1/8, 1:1/32, 2:1/128, 3:1/512)
	CMT0.CMCSR.BIT.CKS = 3;		// ① CKS設定(0:1/8, 1:1/32, 2:1/128, 3:1/512)
	//CMT0.CMCOR = 0xffff;		// ②
	CMT0.CMCOR = 39000;		// ②

	CMT.CMSTR.BIT.STR0 = 1;		// ③ CMT0 カウント開始
	while (1) {
		if (CMT0.CMCSR.BIT.CMF) {	// ④ フラグを確認
			// -- CMCNT == CMCOR 発生 --
			CMT0.CMCSR.BIT.CMF = 0;	// フラグをクリア
			LED5 ^= 1;
			LED6 ^= 1;
		}
	}
}
~~~

~~~c
#include "iodefine.h"

#define  SPK	(PE.DR.BIT.B0)


void main(void)
{
	int even_sec = 0;

	STB.CR4.BIT._CMT = 0;		// モジュールスタンバイ解除

	PFC.PEIORL.BIT.B0 = 1;		// PE0(SPK) .. 出力
	SPK = 0;

	CMT0.CMCSR.BIT.CKS = 0;
	CMT1.CMCSR.BIT.CKS = 3;
	//CMT0.CMCOR = 1250 - 1;
	CMT0.CMCOR = 1000 - 1;
	CMT1.CMCOR = 39000 - 1;

	CMT.CMSTR.BIT.STR0 = 1;
	CMT.CMSTR.BIT.STR1 = 1;
	while (1) {
		if (CMT1.CMCSR.BIT.CMF) {
			even_sec ^= 1;
		}
		if (even_sec && CMT0.CMCSR.BIT.CMF) {
			// -- CMCNT == CMCOR 発生 --
			CMT0.CMCSR.BIT.CMF = 0;	// フラグをクリア
			SPK ^= 1;
		}
	}
}
~~~

seven_LED
--------------

~~~c
#include "iodefine.h"

#define	printf	((int (*)(const char *,...))0x00007c7c)
#define	scanf	((int (*)(const char *,...))0x00007cb8)

#define		DIG1	(PE.DR.BIT.B3)

void init_peior(void);
void init_paior(void);

void main(void) {
	int num;

	init_peior();
	init_paior();

	while (1) {
		printf("input number > "); scanf("%d", &num);
		
		PA.DR.BYTE.HL &= 0xF0;
		PA.DR.BYTE.HL |= num;
		DIG1 = 1;
	}
}

void init_peior(void) {
	PFC.PEIORL.BIT.B3 = 1;
}

void init_paior(void) {
	PFC.PAIORH.BYTE.L |= 0x0F;
}
~~~

~~~c

#include	"iodefine.h"
#include	"typedefine.h"

#define	printf	((int (*)(const char *,...))0x00007c7c)
#define	scanf	((int (*)(const char *,...))0x00007cb8)

#define		DIG1	(PE.DR.BIT.B3)
#define		DIG2	(PE.DR.BIT.B2)
#define		DIG3	(PE.DR.BIT.B1)

#define		SW4		(PD.DR.BIT.B18)
#define		SW5		(PD.DR.BIT.B17)
#define		SW6		(PD.DR.BIT.B16)

#define		WAIT_TM	(10000)

void init_peior(void);
void init_paior(void);
void init_pdior(void);
void digit_disp(_UINT, _UBYTE, _UBYTE);

void main(void) {
	int	cnt;

	init_peior();
	init_paior();
	init_pdior();
	
	PA.DR.BYTE.HL &= 0xF0;
	PA.DR.BYTE.HL |= 0x00;
	
	while (1) {
		if (SW4) {
			while (SW4) ;
			cnt = 0;
			printf("cnt = %d\n", cnt);
		}
		if (SW5) {
			while (SW5) ;
			cnt--;
			if (cnt == -1) cnt = 9;
			printf("cnt = %d\n", cnt);
		}
		if (SW6) {
			while (SW6) ;
			cnt++;
			if (cnt == 10) cnt = 0;
			printf("cnt = %d\n", cnt);
		}
		//digit_disp(WAIT_TM, cnt1, cnt3);
		PA.DR.BYTE.HL &= 0xF0;
		PA.DR.BYTE.HL |= cnt;
		DIG1 = 1;
	}
}

void digit_disp(_UINT tm, _UBYTE data1, _UBYTE data3) {
	_UINT i;
	
	PA.DR.BYTE.HL &= 0xF0;
	PA.DR.BYTE.HL |= data1;
	DIG1 = 0;
	DIG3 = 1;
	for (i = 0; i < tm; i ++) ;
	PA.DR.BYTE.HL &= 0xF0;
	PA.DR.BYTE.HL |= data3;
	DIG3 = 0;
	DIG1 = 1;
	for (i = 0; i < tm; i ++) ;
}

void init_peior(void) {
	PFC.PEIORL.BIT.B1 = 1;
	PFC.PEIORL.BIT.B2 = 1;
	PFC.PEIORL.BIT.B3 = 1;
}

void init_paior(void) {
	PFC.PAIORH.BYTE.L |= 0x0F;
}

void init_pdior(void) {
	PFC.PDIORH.BIT.B16 = 0;
	PFC.PDIORH.BIT.B17 = 0;
	PFC.PDIORH.BIT.B18 = 0;
}
~~~

~~~c
/***********************************************************************/
/*                                                                     */
/*  FILE        :seven_LED_sample3.c                                   */
/*                                                                     */
/***********************************************************************/

#include	"iodefine.h"
#include	"typedefine.h"

#define		DIG1	(PE.DR.BIT.B3)
#define		DIG2	(PE.DR.BIT.B2)

#define		WAIT_TM	(10000)
#define		CNT_TM	(30)

void init_peior(void);
void init_paior(void);
void digit_disp(_UINT, _UBYTE, _UBYTE);

void main(void) {
	_UBYTE	cnt, cnt1, cnt2;
	_UINT	i;
	
	cnt  = 0;
	cnt1 = 0;
	cnt2 = 0;
	
	init_peior();
	init_paior();
	
	PA.DR.BYTE.HL &= 0xF0;
	PA.DR.BYTE.HL |= 0x00;
	
	while(1) {
		cnt ++;
		if(cnt == 100) {
			cnt = 0;
		}
		cnt2 = cnt / 10;
		cnt1 = cnt % 10;
		for(i = 0; i < CNT_TM; i++) {
			digit_disp(WAIT_TM, cnt1, cnt2);
		}
	}
}

void digit_disp(_UINT tm, _UBYTE data1, _UBYTE data2) {
	_UINT i;
	
	PA.DR.BYTE.HL &= 0xF0;
	PA.DR.BYTE.HL |= data1;
	DIG1 = 1;
	DIG2 = 0;
	for(i = 0; i < tm; i ++);
	PA.DR.BYTE.HL &= 0xF0;
	PA.DR.BYTE.HL |= data2;
	DIG2 = 1;
	DIG1 = 0;
	for(i = 0; i < tm; i ++);
}

void init_peior(void) {
	PFC.PEIORL.BIT.B1 = 1;
	PFC.PEIORL.BIT.B2 = 1;
}

void init_paior(void) {
	PFC.PAIORH.BYTE.L |= 0x0F;
}
~~~

LCD
----------------

~~~c
/*
 * LCD_prog1.c
 *
 */

#include <machine.h>
#include "iodefine.h"

#define LCD_RS		(PA.DR.BIT.B22)
#define LCD_E		(PA.DR.BIT.B23)
#define LCD_RW		(PD.DR.BIT.B23)
#define LCD_DATA	(PD.DR.BYTE.HH)


#define	printf	((int (*)(const char *,...))0x00007c7c)
#define	scanf	((int (*)(const char *,...))0x00007cb8)


void wait_us(unsigned int);
void LCD_inst(char);
void LCD_data(char);

/********************************************************/
void LCD_init() {
	wait_us(45000);
	LCD_inst(0x30);
	wait_us(4100);
	LCD_inst(0x30);
	wait_us(100);
	LCD_inst(0x30);
	
	LCD_inst(0x38);
	LCD_inst(0x08);
	LCD_inst(0x01);
	wait_us(1640);
	LCD_inst(0x06);
	LCD_inst(0x0c);
}
/********************************************************/
void CMT0_init() {
	STB.CR4.BIT._CMT = 0;		// モジュールスタンバイ解除

	CMT0.CMCSR.BIT.CKS = 1;
}

int min(unsigned long a, unsigned long b) {
	if (a < b)
		return a;
	else
		return b;
}

int max(unsigned long a, unsigned long b) {
	if (a > b)
		return a;
	else
		return b;
}


void wait_us(unsigned int us) {
	
	us = max(us, 4);
	CMT0.CMCOR = min(0xffff, us * 10 / 16 - 1);

	CMT.CMSTR.BIT.STR0 = 1;
	
	while (CMT0.CMCSR.BIT.CMF == 0);
	
	CMT0.CMCSR.BIT.CMF = 0;
	CMT.CMSTR.BIT.STR0 = 0;
	
}
/********************************************************/
void LCD_cursor(unsigned int x, unsigned int y) {
	x &= 0x0f;
	y &= 0x01;
	LCD_inst(0x80 | x | y << 6);
}
/********************************************************/
void LCD_putch(char ch) {
	LCD_data(ch);
}
/********************************************************/
void LCD_putstr(char *str) {
	while (*str) {
		LCD_putch(*str);
		str++;
	}
}
/********************************************************/
/* LCD モジュールにインストラクションコードを書き込む */
/********************************************************/
void LCD_inst(char inst) {
	LCD_E = 0;
	LCD_RS = 0;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = inst;
	LCD_E = 0;
	wait_us(40);
}
/********************************************************/
/* LCD モジュールにデータを書き込む */
/********************************************************/
void LCD_data(char data)
{
	LCD_E = 0;
	LCD_RS = 1;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = data;
	LCD_E = 0;
	wait_us(40);
}
/********************************************************/
void main() {
	char buf[128] = { 0 };
	unsigned int x, y;	
	
	PFC.PAIORH.BIT.B22 = 1;
	PFC.PAIORH.BIT.B23 = 1;
	PFC.PDIORH.BIT.B23 = 1;
	PFC.PDIORH.BYTE.H = 0xff;

	STB.CR4.BIT._CMT = 0;
	CMT0.CMCSR.BIT.CKS = 1;

	CMT0_init();
	LCD_init();

/*
	LCD_inst(0x8f);
	LCD_data('A');

	LCD_cursor(8, 1);
	LCD_putch('B');
	LCD_cursor(10, 1);
	LCD_putstr("CDEF");
*/

	while (1) {
		printf("X Y String : ");
		scanf("%d %d %s", &x, &y, buf);
		//LCD_inst(0x01);
		LCD_cursor(x, y);
		LCD_putstr(buf);
	}
}
~~~

~~~c
/*
 * LCD_prog2.c
 *
 */

#include "iodefine.h"

#define printf	((int (*)(const char*, ...))0x00007c7c)
#define scanf	((int (*)(const char*, ...))0x00007cb8)

#define LCD_RS		(PA.DR.BIT.B22)
#define LCD_E		(PA.DR.BIT.B23)
#define LCD_RW		(PD.DR.BIT.B23)
#define LCD_DATA	(PD.DR.BYTE.HH)

/********************************************************/
void LCD_init() {
	wait_us(45000);
	LCD_inst(0x30);
	wait_us(4100);
	LCD_inst(0x30);
	wait_us(100);
	LCD_inst(0x30);
	
	LCD_inst(0x38);
	LCD_inst(0x08);
	LCD_inst(0x01);
	wait_us(1640);
	LCD_inst(0x06);
	LCD_inst(0x0c);
}
/********************************************************/
void wait_us(unsigned int us) {
	unsigned int val;

	val = us * 10 / 16;
	if (val >= 0xffff)
		val = 0xffff;

	CMT0.CMCOR = val;
	CMT0.CMCSR.BIT.CMF &= 0;
	CMT.CMSTR.BIT.STR0 = 1;

	while (CMT0.CMCSR.BIT.CMF == 0)
		;
	CMT0.CMCSR.BIT.CMF = 0;
	CMT.CMSTR.BIT.STR0 = 0;
}
/********************************************************/
void LCD_cursor(unsigned int x, unsigned int y) {
	if (x > 15)
		x = 15;

	if (y > 1)
		y = 1;

	LCD_inst(0x80 | x | y << 6);
}
/********************************************************/
void LCD_putch(char ch) {
	LCD_data(ch);
}
/********************************************************/
void LCD_putstr(char *str) {
	char ch;

	while (ch = *str++)
		LCD_putch(ch);
}
/********************************************************/
/* LCD モジュールにインストラクションコードを書き込む */
/********************************************************/
void LCD_inst(char inst) {
	LCD_E = 0;
	LCD_RS = 0;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = inst;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}
/********************************************************/
/* LCD モジュールにデータを書き込む */
/********************************************************/
void LCD_data(char data) {
	LCD_E = 0;
	LCD_RS = 1;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = data;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}
/********************************************************/
void LCD_cls() {
	LCD_inst(0x01);
	wait_us(1640);
}
/********************************************************/
void main() {
	unsigned int x, y;
	char str[16];

	PFC.PAIORH.BIT.B22 = 1;
	PFC.PAIORH.BIT.B23 = 1;
	PFC.PDIORH.BIT.B23 = 1;
	PFC.PDIORH.BYTE.H = 0xff;

	STB.CR4.BIT._CMT = 0;
	CMT0.CMCSR.BIT.CKS = 1;
	CMT1.CMCSR.BIT.CKS = 3;
	CMT1.CMCOR = 19530;

	LCD_init();

	while (1) {
		printf("X Y String : ");
		scanf("%d %d %s", &x, &y, str);

		/* 必要な処理を記述 */

	}
}
~~~

abc_single
------------------

~~~c
/***********************************************************************/
/*  A/D変換器のシングルスキャンモードを使った演習                      */
/***********************************************************************/

#include <machine.h>
#include "iodefine.h"
#include "typedefine.h"

#define ex5

#define	printf	((int (*)(const char *,...))0x00007c7c)

#define	SW6 (PD.DR.BIT.B18)
#define	SW4 (PD.DR.BIT.B16)

// #ifdef ex1
void wait_us(unsigned int);
// #endif

#ifdef ex6
#define  DIG1  (PE.DR.BIT.B3)
#define  DIG2  (PE.DR.BIT.B2)
#define  DIG3  (PE.DR.BIT.B1)
void disp_led(_SINT, _UINT);
#endif

#ifdef ex7
#define LCD_RS    (PA.DR.BIT.B22)
#define LCD_E     (PA.DR.BIT.B23)
#define LCD_RW    (PD.DR.BIT.B23)
#define LCD_DATA  (PD.DR.BYTE.HH)

// void wait_us(_UINT);
void LCD_inst(_SBYTE);
void LCD_data(_SBYTE);
void LCD_cursor(_UINT, _UINT);
void LCD_putch(_SBYTE);
void LCD_putstr(_SBYTE *);
void LCD_cls(void);
void LCD_init(void);
#endif

void main(void) {
#ifdef ex1
	while (1) {
		STB.CR4.BIT._AD0 = 0;
		AD0.ADCR.BIT.ADST = 1;
		while (AD0.ADCR.BIT.ADST)
			;
		AD0.ADCSR.BIT.ADF = 0;
		printf("ADDR0 = %04d \n",  AD0.ADDR0 >> 6);
		wait_us(100000);
	}
	// while (1);
#endif

#ifdef ex2
	while (1) {
		STB.CR4.BIT._AD0 = 0;
		AD0.ADCR.BIT.ADST = 1;
		AD0.ADCSR.BIT.CH = 1;
		while (AD0.ADCR.BIT.ADST)
			;
		AD0.ADCSR.BIT.ADF = 0;
		printf("ADDR1 = %04d \n",  AD0.ADDR1 >> 6);
		wait_us(100000);
	}
	// while (1);
#endif

#ifdef ex3
	while (1) {
		STB.CR4.BIT._AD0 = 0;
		AD0.ADCR.BIT.ADST = 1;
		while (!AD0.ADCSR.BIT.ADF)
			;
		AD0.ADCSR.BIT.ADF = 0;
		printf("ADDR0 = %04d \n",  AD0.ADDR0 >> 6);
		wait_us(100000);
	}
	// while (1);
#endif

#ifdef ex4
	PFC.PEIORL.WORD |= 0x0800;
	PFC.PDIORH.WORD &= 0xfffB;
	PE.DR.WORD.L |= 0x0800;

	STB.CR4.BIT._AD0 = 0;
	AD0.ADCSR.BIT.CH = 0;
	
	while (1) {
		if (SW6) {
			while (SW6)
				;
			AD0.ADCR.BIT.ADST = 1;
			while (AD0.ADCR.BIT.ADST)
				;
			AD0.ADCSR.BIT.ADF = 0;
			printf("ADDR0 = %04d \n", AD0.ADDR0 >> 6);
		}
	}
#endif

#ifdef ex5
	PFC.PEIORL.BIT.B9 = 1;  // sw4
	PFC.PEIORL.BIT.B11 = 1; // sw6
	PFC.PDIORH.BIT.B16 = 0; // sw4
	PFC.PDIORH.BIT.B18 = 0; // sw6

	PE.DR.WORD.L |= 0x0800;
	
	while (1) {
		STB.CR4.BIT._AD0 = 0;
		if (SW6) {
			while (SW6)
				;
			AD0.ADCSR.BIT.CH = 0;
			AD0.ADCR.BIT.ADST = 1;
			while (!AD0.ADCSR.BIT.ADF)
				;
			AD0.ADCSR.BIT.ADF = 0;
			printf("ADDR0 = %04d \n", AD0.ADDR0 >> 6);
		}
		if (SW4) {
			while (SW4)
				;
			AD0.ADCSR.BIT.CH = 1;
			AD0.ADCR.BIT.ADST = 1;
			while (!AD0.ADCSR.BIT.ADF)
				;
			AD0.ADCSR.BIT.ADF = 0;
			printf("ADDR1 = %04d \n", AD0.ADDR1 >> 6);
		}
	}
#endif

#ifdef ex6
	_UWORD	ad_rst = 0;
	_SINT	keta = 0;
	STB.CR4.BIT._AD0 = 0;
	STB.CR4.BIT._CMT = 0;
#endif

#ifdef ex7
	_UWORD	ad_rst = 0;
	_SBYTE	str[16] = {' ',' ','A','D','D','R','0',' ','=',' ','0','0','0',' ',' ','\0'};
	STB.CR4.BIT._AD0 = 0;
	STB.CR4.BIT._CMT = 0;
#endif

}

// #ifdef ex1
void wait_us(unsigned int us) {
	for (; us > 0; us--) {
		nop();
		nop();
		nop();
		nop();
	}
}
// #endif

#ifdef ex6
void disp_led(_SINT digit, _UINT ad_rst) {
	DIG1 = DIG2 = DIG3 = 0;
	PA.DR.BYTE.HL &= 0xf0;
	if (digit == 0) {
		PA.DR.BYTE.HL |= ad_rst % 10;
		DIG1 = 1;
	} else if (digit == 1) {
		PA.DR.BYTE.HL |= (ad_rst / 10) % 10;
		DIG2 = 1;
	} else {
		PA.DR.BYTE.HL |= (ad_rst / 100) % 10;
		DIG3 = 1;
	}
}
#endif

#ifdef ex7
void wait_us(_UINT us) {
	_UINT val;

	val = us * 10 / 16;
	if (val >= 0xffff)
		val = 0xffff;

	CMT0.CMCOR = val;
	CMT0.CMCSR.BIT.CMF &= 0;
	CMT.CMSTR.BIT.STR0 = 1;
	while (!CMT0.CMCSR.BIT.CMF);
	CMT0.CMCSR.BIT.CMF = 0;
	CMT.CMSTR.BIT.STR0 = 0;
}

void LCD_inst(_SBYTE inst) {
	LCD_E = 0;
	LCD_RS = 0;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = inst;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_data(_SBYTE data) {
	LCD_E = 0;
	LCD_RS = 1;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = data;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_cursor(_UINT x, _UINT y) {
	if (x > 15)
		x = 15;
	if (y > 1)
		y = 1;
	LCD_inst(0x80 | x | y << 6);
}

void LCD_putch(_SBYTE ch) {
	LCD_data(ch);
}

void LCD_putstr(_SBYTE *str) {
	_SBYTE ch;

	while (ch = *str++)
		LCD_putch(ch);
}

void LCD_cls(void) {
	LCD_inst(0x01);
	wait_us(1640);
}

void LCD_init(void) {
	wait_us(45000);
	LCD_inst(0x30);
	wait_us(4100);
	LCD_inst(0x30);
	wait_us(100);
	LCD_inst(0x30);
	
	LCD_inst(0x38);
	LCD_inst(0x08);
	LCD_inst(0x01);
	wait_us(1640);
	LCD_inst(0x06);
	LCD_inst(0x0c);
}
#endif

~~~

abc_1scan
---------------

~~~c
/***********************************************************************/
/*  A/D変換器の１サイクルスキャンモードを使った演習                    */
/***********************************************************************/
#include <machine.h>
#include "iodefine.h"
#include "typedefine.h"

#define ex4

#define	printf	((int (*)(const char *,...))0x00007c7c)
#define	SW6 (PD.DR.BIT.B18)

#ifdef ex4
#define LCD_RS		(PA.DR.BIT.B22)
#define LCD_E		(PA.DR.BIT.B23)
#define LCD_RW		(PD.DR.BIT.B23)
#define LCD_DATA	(PD.DR.BYTE.HH)

void wait_us(_UINT);
void LCD_inst(_SBYTE);
void LCD_data(_SBYTE);
void LCD_cursor(_UINT, _UINT);
void LCD_putch(_SBYTE);
void LCD_putstr(_SBYTE *);
void LCD_cls(void);
void LCD_init(void);
#endif

void main(void);

void main(void) {
#ifdef ex1
	STB.CR4.BIT._AD0 = 0;
	AD0.ADCSR.BIT.ADM = 3;
	AD0.ADCSR.BIT.CH = 1;
	AD0.ADCR.BIT.ADST = 1;
	while (AD0.ADCR.BIT.ADST)
		;
	AD0.ADCSR.BIT.ADF = 0;
	printf("ADDR0 = %04d\nADDR1 = %04d\n", AD0.ADDR0 >> 6, AD0.ADDR1 >> 6);
	while (1)
		;
#endif

#ifdef ex2
	STB.CR4.BIT._AD0 = 0;
	AD0.ADCSR.BIT.ADM = 3;
	AD0.ADCSR.BIT.CH = 1;
	AD0.ADCR.BIT.ADST = 1;
	while (!AD0.ADCSR.BIT.ADF)
		;
	AD0.ADCSR.BIT.ADF = 0;
	printf("ADDR0 = %04d\nADDR1 = %04d\n", AD0.ADDR0 >> 6, AD0.ADDR1 >> 6);
	while (1)
		;
#endif

#ifdef ex3
	PFC.PEIORL.WORD |= 0x0800;
	PFC.PDIORH.WORD &= 0xfffB;
	PE.DR.WORD.L |= 0x0800;

	STB.CR4.BIT._AD0 = 0;
	AD0.ADCSR.BIT.ADM = 3;
	AD0.ADCSR.BIT.CH = 1;

	while (1) {
		if (SW6) {
			while (SW6)
				;
			AD0.ADCR.BIT.ADST = 1;
			while (AD0.ADCR.BIT.ADST)
				;
			AD0.ADCSR.BIT.ADF = 0;
			printf("ADDR0 = %04d\nADDR1 = %04d\n", AD0.ADDR0 >> 6, AD0.ADDR1 >> 6);
		}
	}

#endif

#ifdef ex4
	_UWORD	ad_rst = 0;
	_SBYTE	str[16] = {
		' ',' ','A','D','D','R','0',' ','=',' ','0','0','0',' ',' ','\0'
	};
	int ad0, ad1;
	int d2, d1, d0;

	PFC.PEIORL.WORD |= 0x0800;
	PFC.PDIORH.WORD &= 0xfffB;
	PE.DR.WORD.L |= 0x0800;

	STB.CR4.BIT._AD0 = 0;
	STB.CR4.BIT._CMT = 0;

	AD0.ADCSR.BIT.ADM = 3;
	AD0.ADCSR.BIT.CH = 1;
	
	while (1) {
		if (SW6) {
			while (SW6)
				;
			AD0.ADCR.BIT.ADST = 1;
			while (!AD0.ADCSR.BIT.ADF)
				;
			AD0.ADCSR.BIT.ADF = 0;

			LCD_init();
			str[6] = '0';
			LCD_cursor(0, 0);
			LCD_putstr(str);
			str[6] = '1';
			LCD_cursor(0, 1);
			LCD_putstr(str);
			// LCD_cursor(6, 1);
			// LCD_putch('1');

			ad0 = AD0.ADDR0 >> 6;
			d0 = ad0 % 10; ad0 /= 10;
			d1 = ad0 % 10; ad0 /= 10;
			d2 = ad0 % 10;

			printf("%d %d %d\n", d2, d1, d0);

			LCD_cursor(10, 0);
			LCD_putch('0' + d2);
			LCD_cursor(11, 0);
			LCD_putch('0' + d1);
			LCD_cursor(12, 0);
			LCD_putch('0' + d0);

			ad1 = AD0.ADDR1 >> 6;
			d0 = ad1 % 10; ad1 /= 10;
			d1 = ad1 % 10; ad1 /= 10;
			d2 = ad1 % 10;

			printf("%d %d %d\n", d2, d1, d0);

			LCD_cursor(10, 1);
			LCD_putch('0' + d2);
			LCD_cursor(11, 1);
			LCD_putch('0' + d1);
			LCD_cursor(12, 1);
			LCD_putch('0' + d0);
		}
	}

	while (1)
		;
#endif
}

#ifdef ex4
void wait_us(unsigned int us) {
	for (; us > 0; us--) {
		nop();
		nop();
		nop();
		nop();
	}
}
// void wait_us(_UINT us) {
// 	_UINT val;

// 	val = us * 10 / 16;
// 	if (val >= 0xffff)
// 		val = 0xffff;

// 	CMT0.CMCOR = val;
// 	CMT0.CMCSR.BIT.CMF &= 0;
// 	CMT.CMSTR.BIT.STR0 = 1;
// 	while (!CMT0.CMCSR.BIT.CMF);
// 	CMT0.CMCSR.BIT.CMF = 0;
// 	CMT.CMSTR.BIT.STR0 = 0;
// }

void LCD_inst(_SBYTE inst) {
	LCD_E = 0;
	LCD_RS = 0;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = inst;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_data(_SBYTE data) {
	LCD_E = 0;
	LCD_RS = 1;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = data;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_cursor(_UINT x, _UINT y) {
	if (x > 15)
		x = 15;
	if (y > 1)
		y = 1;
	LCD_inst(0x80 | x | y << 6);
}

void LCD_putch(_SBYTE ch) {
	LCD_data(ch);
}

void LCD_putstr(_SBYTE *str) {
	_SBYTE ch;

	while (ch = *str++)
		LCD_putch(ch);
}

void LCD_cls(void) {
	LCD_inst(0x01);
	wait_us(1640);
}

void LCD_init(void) {
	wait_us(45000);
	LCD_inst(0x30);
	wait_us(4100);
	LCD_inst(0x30);
	wait_us(100);
	LCD_inst(0x30);
	
	LCD_inst(0x38);
	LCD_inst(0x08);
	LCD_inst(0x01);
	wait_us(1640);
	LCD_inst(0x06);
	LCD_inst(0x0c);
}
#endif
~~~

abc_nscan
------------------

~~~c
/***********************************************************************/
/*  A/D変換器の連続スキャンモードを使った演習                    */
/***********************************************************************/

#include "iodefine.h"
#include "typedefine.h"

#define ex2

#define	printf	((int (*)(const char *,...))0x00007c7c)
#define	SW6 (PD.DR.BIT.B18)
#define	SW4 (PD.DR.BIT.B16)

#ifdef ex3
#define LCD_RS		(PA.DR.BIT.B22)
#define LCD_E		(PA.DR.BIT.B23)
#define LCD_RW		(PD.DR.BIT.B23)
#define LCD_DATA	(PD.DR.BYTE.HH)

void wait_us(_UINT);
void LCD_inst(_SBYTE);
void LCD_data(_SBYTE);
void LCD_cursor(_UINT, _UINT);
void LCD_putch(_SBYTE);
void LCD_putstr(_SBYTE *);
void LCD_cls(void);
void LCD_init(void);
#endif

#ifdef ex4
#define LCD_RS		(PA.DR.BIT.B22)
#define LCD_E		(PA.DR.BIT.B23)
#define LCD_RW		(PD.DR.BIT.B23)
#define LCD_DATA	(PD.DR.BYTE.HH)

void wait_us(_UINT);
void LCD_inst(_SBYTE);
void LCD_data(_SBYTE);
void LCD_cursor(_UINT, _UINT);
void LCD_putch(_SBYTE);
void LCD_putstr(_SBYTE *);
void LCD_cls(void);
void LCD_init(void);
#endif

void main(void);

void main(void) {

#ifdef ex1
	STB.CR4.BIT._AD0 = 0;
	AD0.ADCSR.BIT.ADM = 3;
	AD0.ADCSR.BIT.ADCS = 1;
	AD0.ADCSR.BIT.CH = 1;
	AD0.ADCR.BIT.ADST = 1;
	while(1) {
		while(!AD0.ADCSR.BIT.ADF);
		AD0.ADCSR.BIT.ADF = 0;
		printf("ADDR0 = %04d\nADDR1 = %04d\n",
									  AD0.ADDR0 >> 6, AD0.ADDR1 >> 6);
	}

#endif

#ifdef ex2
	STB.CR4.BIT._AD0 = 0;
	AD0.ADCSR.BIT.ADM = 3;
	AD0.ADCSR.BIT.ADCS = 1;
	AD0.ADCSR.BIT.CH = 1;
	while(1) {
		if (SW6) {
			AD0.ADCR.BIT.ADST = 1;
			AD0.ADCSR.BIT.ADF = 0;
			while (1) {
				while(!AD0.ADCSR.BIT.ADF);
				AD0.ADCSR.BIT.ADF = 0;
				printf("ADDR0 = %04d\nADDR1 = %04d\n",
									  AD0.ADDR0 >> 6, AD0.ADDR1 >> 6);
				if (SW4)
					break;
			}
			AD0.ADCR.BIT.ADST = 0;
			AD0.ADCSR.BIT.ADF = 1;
		}
	}

#endif

#ifdef ex3
	_UWORD	ad_rst = 0;
	_SBYTE	str[16] = {' ',' ','A','D','D','R','0',' ','=',' ','0','0','0',' ',' ','\0'};
	STB.CR4.BIT._AD0 = 0;
	STB.CR4.BIT._CMT = 0;

#endif

#ifdef ex4
	_UWORD	ad_rst = 0;
	_SBYTE	str[16] = {' ',' ','A','D','D','R','0',' ','=',' ','0','0','0',' ',' ','\0'};
	STB.CR4.BIT._AD0 = 0;
	STB.CR4.BIT._CMT = 0;

#endif

}

#ifdef ex3
void wait_us(_UINT us) {
	_UINT val;

	val = us * 10 / 16;
	if (val >= 0xffff)
		val = 0xffff;

	CMT0.CMCOR = val;
	CMT0.CMCSR.BIT.CMF &= 0;
	CMT.CMSTR.BIT.STR0 = 1;
	while (!CMT0.CMCSR.BIT.CMF);
	CMT0.CMCSR.BIT.CMF = 0;
	CMT.CMSTR.BIT.STR0 = 0;
}

void LCD_inst(_SBYTE inst) {
	LCD_E = 0;
	LCD_RS = 0;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = inst;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_data(_SBYTE data) {
	LCD_E = 0;
	LCD_RS = 1;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = data;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_cursor(_UINT x, _UINT y) {
	if (x > 15)
		x = 15;
	if (y > 1)
		y = 1;
	LCD_inst(0x80 | x | y << 6);
}

void LCD_putch(_SBYTE ch) {
	LCD_data(ch);
}

void LCD_putstr(_SBYTE *str) {
	_SBYTE ch;

	while (ch = *str++)
		LCD_putch(ch);
}

void LCD_cls(void) {
	LCD_inst(0x01);
	wait_us(1640);
}

void LCD_init(void) {
	wait_us(45000);
	LCD_inst(0x30);
	wait_us(4100);
	LCD_inst(0x30);
	wait_us(100);
	LCD_inst(0x30);
	
	LCD_inst(0x38);
	LCD_inst(0x08);
	LCD_inst(0x01);
	wait_us(1640);
	LCD_inst(0x06);
	LCD_inst(0x0c);
}
#endif

#ifdef ex4
void wait_us(_UINT us) {
	_UINT val;

	val = us * 10 / 16;
	if (val >= 0xffff)
		val = 0xffff;

	CMT0.CMCOR = val;
	CMT0.CMCSR.BIT.CMF &= 0;
	CMT.CMSTR.BIT.STR0 = 1;
	while (!CMT0.CMCSR.BIT.CMF);
	CMT0.CMCSR.BIT.CMF = 0;
	CMT.CMSTR.BIT.STR0 = 0;
}

void LCD_inst(_SBYTE inst) {
	LCD_E = 0;
	LCD_RS = 0;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = inst;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_data(_SBYTE data) {
	LCD_E = 0;
	LCD_RS = 1;
	LCD_RW = 0;
	LCD_E = 1;
	LCD_DATA = data;
	wait_us(1);
	LCD_E = 0;
	wait_us(40);
}

void LCD_cursor(_UINT x, _UINT y) {
	if (x > 15)
		x = 15;
	if (y > 1)
		y = 1;
	LCD_inst(0x80 | x | y << 6);
}

void LCD_putch(_SBYTE ch) {
	LCD_data(ch);
}

void LCD_putstr(_SBYTE *str) {
	_SBYTE ch;

	while (ch = *str++)
		LCD_putch(ch);
}

void LCD_cls(void) {
	LCD_inst(0x01);
	wait_us(1640);
}

void LCD_init(void) {
	wait_us(45000);
	LCD_inst(0x30);
	wait_us(4100);
	LCD_inst(0x30);
	wait_us(100);
	LCD_inst(0x30);
	
	LCD_inst(0x38);
	LCD_inst(0x08);
	LCD_inst(0x01);
	wait_us(1640);
	LCD_inst(0x06);
	LCD_inst(0x0c);
}
#endif

~~~



