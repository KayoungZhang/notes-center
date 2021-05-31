
.. 标题文字下的符号长度都要大于标题长度

32位数据位操作基本方式
==========================

1.获取32bit数据单字节::

	#define GET_BYTE0(x)	((x >> 0) & 0x000000FF)
	#define GET_BYTE1(x)	((x >> 8) & 0x000000FF)
	#define GET_BYTE2(x)	((x >> 16) & 0x000000FF)
	#define GET_BYTE3(x)	((x >> 24) & 0x000000FF)
	
2. 获取32bit数据某一位::

  #define GET_BIT(x, n)	((x & (1<<n)) >> n) 

3. 获取32bit数据某连续几位的值::

	/* 获取第[n:m]位的值(m>n) */
	#define GET_BITS(x, n, m)  ((x << (31-m)) >> (31-(m-n)))
	/* 获取第[n:m]位的值(m<n) */
	#define GET_BITS(x, n, m)  ((x << (31-n)) >> (31-(n-m)))
	
4.32bit数据清零某一字节::

	#define CLEAR_BYTE0(x)  (x & 0xFFFFFF00)
	#define CLEAR_BYTE1(x)  (x & 0xFFFF00FF)
	#define CLEAR_BYTE2(x)  (x & 0xFF00FFFF)
	#define CLEAR_BYTE3(x)  (x & 0x00FFFFFF)
	
5. 32bit数据清零某一位::

  #define CLEAR_BIT(x, n)    (x & ~(1<<n))
	
6. 32bit数据某一字节置位::

	#define SET_BYTE0(x)	x | 0x000000FF
	#define SET_BYTE1(x)	x | 0x0000FF00
	#define SET_BYTE2(x)	x | 0x00FF0000
	#define SET_BYTE3(x)	x | 0xFF000000
	
7. 32bit数据某一位置位::

  #define SET_BIT(x, n)     x | (1<<n)

-  **实例** ::

	#include <stdio.h>
	/*1.获取32bit数据单字节*/
	// #define GET_BYTE0(x)	((x >> 0) & 0x000000FF)
	// #define GET_BYTE1(x)	((x >> 8) & 0x000000FF)
	// #define GET_BYTE2(x)	((x >> 16) & 0x000000FF)
	// #define GET_BYTE3(x)	((x >> 24) & 0x000000FF)

	// int main(void)
	// {
		// unsigned int n = 0x12345678;
		// printf("0x%x\r\n", GET_BYTE0(n)); //0x78
		// printf("0x%x\r\n", GET_BYTE1(n)); //0x56
		// printf("0x%x\r\n", GET_BYTE2(n)); //0x34
		// printf("0x%x\r\n", GET_BYTE3(n)); //0x12
			
		// return 0;
	// }

	/*2. 获取32bit数据某一位*/
	// #define GET_BIT(x, n)	((x & (1<<n)) >> n) 

	// int main(void)
	// {
		// unsigned int a = 0x12345678;//0001_0010_0011_0100_0101_0110_0111_1000
		
		// printf("bit0 is %d\r\n", GET_BIT(a, 0));//0
		// printf("bit3 is %d\r\n", GET_BIT(a, 3));//1
		// printf("bit28 is %d\r\n", GET_BIT(a, 28));//1
		// printf("bit31 is %d\r\n", GET_BIT(a, 31));//0
		// return 0;
	// }

	/*3. 获取32bit数据某连续几位的值*/
	// /* 获取第[n:m]位的值m>n */
	// #define GET_BITS(x, n, m)  ((x << (31-m)) >> (31-(m-n)))
	/* 获取第[n:m]位的值m<n */
	#define GET_BITS(x, n, m)  ((x << (31-n)) >> (31-(n-m)))
	int main(void)
	{
		unsigned int a = 0x12345678;//0001_0010_0011_0100_0101_0110_0111_1000
		printf("bit[3, 1] is %x\r\n", GET_BITS(a, 3, 1));//0100b
		return 0;
	}

	/*4.32bit数据清零某一字节*/
	// #define CLEAR_BYTE0(x)  (x & 0xFFFFFF00)
	// #define CLEAR_BYTE1(x)  (x & 0xFFFF00FF)
	// #define CLEAR_BYTE2(x)  (x & 0xFF00FFFF)
	// #define CLEAR_BYTE3(x)  (x & 0x00FFFFFF)

	// int main(void)
	// {
		// unsigned int a = 0x12345678;
		// printf("0x%x\r\n", CLEAR_BYTE0(a));//0x12345600
		// printf("0x%x\r\n", CLEAR_BYTE1(a));//0x12340078
		// printf("0x%x\r\n", CLEAR_BYTE2(a));//0x12005678
		// printf("%#.8x\r\n", CLEAR_BYTE3(a));//0x00345678
		// return 0;
	// }

	/*5. 32bit数据清零某一位*/
	// #define CLEAR_BIT(x, n)    (x & ~(1<<n))

	// int main(void)
	// {
		// unsigned int a = 0x12345678;//0001_0010_0011_0100_0101_0110_0111_1000
		// printf("bit3 change to 0. 0x%x\r\n", CLEAR_BIT(a, 3));//0x12345670
		// printf("bit6 change to 0. 0x%x\r\n", CLEAR_BIT(a, 6));//0x12345638
		// printf("bit28 change to 0. 0x%x\r\n", CLEAR_BIT(a, 28));//0x2345678
		// printf("bit31 change to 0. 0x%x\r\n", CLEAR_BIT(a, 31));//0x12345678
		// return 0;
	// }

	/*6. 32bit数据某一字节置位*/
	// #define SET_BYTE0(x)	x | 0x000000FF
	// #define SET_BYTE1(x)	x | 0x0000FF00
	// #define SET_BYTE2(x)	x | 0x00FF0000
	// #define SET_BYTE3(x)	x | 0xFF000000

	// int main(void)
	// {
		// unsigned int a = 0x12345678;
		// printf("set byte0: 0x%x\r\n", SET_BYTE0(a));//0x123456ff
		// printf("set byte1: 0x%x\r\n", SET_BYTE1(a));//0x1234ff78
		// printf("set byte2: 0x%x\r\n", SET_BYTE2(a));//0x12ff5678
		// printf("set byte3: 0x%x\r\n", SET_BYTE3(a));//0xff345678
		// return 0;
	// }

	/*7. 32bit数据某一位置位*/
	// #define SET_BIT(x, n)     x | (1<<n)

	// int main(void)
	// {
		// unsigned int a = 0x12345678;//0001_0010_0011_0100_0101_0110_0111_1000
		// printf("set bit0 to 1: 0x%x\r\n", SET_BIT(a, 0));//0x12345679
		// printf("set bit3 to 1: 0x%x\r\n", SET_BIT(a, 3));//0x12345678
		// printf("set bit7 to 1: 0x%x\r\n", SET_BIT(a, 7));//0x123456f8
		// printf("set bit31 to 1: 0x%x\r\n", SET_BIT(a, 31));//0x92345678
		// return 0;
	// }

