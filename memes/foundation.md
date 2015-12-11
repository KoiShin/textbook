
MEMEs基礎
============

htermでの入出力
--------------

マイコンのMEMEsとパソコンのhterm間で入出力を行うためには、printf、scanfを用いる。
printfとscanf関数は、MEMEsのメモリ上にあるので、そのアドレスをマクロとして利用する。

~~~ c
#define printf ((int (*)(const char *,...))0x00007c7c)
#define scanf  ((int (*)(const char *,...))0x00007cb8)
~~~

iodefine.h
--------------


