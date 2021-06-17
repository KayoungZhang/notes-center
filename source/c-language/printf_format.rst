
.. 标题文字下的符号长度都要大于标题长度

printf总结
==========================

printf函数声明于 `<stdio.h>` 头文件中。

* 函数原型：
 `int printf ( const char * format, ... );` 返回值：正确返回输出的字符总数，错误返回负值。
 
* 调用格式：
 `printf("格式化字符串", 输出表列);` 
  格式化字符串包含：（1）字符串常量；（2）格式控制字符串；（3）转义字符。

* 格式控制字符串组成::

	%[flags][width][.prec][length]type  //%[标志][最小宽度][.精度][类型长度]类型。
	有符号十进制:	printf("%d",123);//输出123
	无符号十进制:	printf("%u",123);//输出123
	无符号八进制:	printf("%o",123);//输出123
	无符号十六进制：	printf("0x%x 0x%X",123,123);  //输出0x7b 0x7B
	无符号十六进制：	printf("%#x %#X",123,123);  //输出0x7b 0X7B
	字符型数据：	printf("%c",64);//输出A
	字符串型数据：	printf("%s","测试test");//输出：测试test
	单精度浮点数：	printf("%.9f",0.000000123);//输出0.000000123
	科学计数法：	printf("%e %E",0.000000123,0.000000123);//输出1.230000e-07 1.230000E-07

	printf("%5d\n",1000);     //输出:( 1000)默认右对齐,左边补空格
	printf("%-5d\n",1000);     //输出:(1000 )左对齐,右边补空格
	printf("%05d\n",1000);    //输出:(01000)前面补0
	printf("%#x\n",1000);  //输出:0x3e8
	printf("%.0f %#.0f\n",1000.0,1000.0);//输出:(1000 1000.)当小数点后不输出值时依然输出小数点

* 编译器内置的几个宏定义
ANSI C标准中有几个标准预定义宏（也是常用的）::

	__LINE__：当前的函数行 (int)
	__FILE__：当前程序源文件 (char*)
	__DATE__：在源文件中插入当前的编译日期
	__TIME__：在源文件中插入当前编译时间
	__STDC__：当要求程序严格遵循ANSI C标准时该标识被赋值为1
	__FUNCTION__:  当前运行的函数 (char*)
	__cplusplus：当编写C++程序时该标识符被定义。

eg::
	
	printf("file: %s\n", __FILE__);
    printf("function: %s\n", __FUNCTION__);
    printf("line: %d\n", __LINE__);
	
	
* # 字符串化操作符
在gcc的编译系统中，可以使用#将当前的内容转换成字符串。

.. code-block:: C

	#include <stdio.h>
	#define DPRINT(expr) printf("%s = %d\n", #expr, expr);
	int main(void)
	{
		int x = 3;
		int y = 5;

		DPRINT(x / y);
		DPRINT(x + y);
		DPRINT(x * y);
		
		return 0;
	}

执行结果::

	x / y = 0
	x + y = 8
	x * y = 15
	
* ## 连接操作符	
在gcc的编译系统中，##是C语言中的连接操作符，可以在编译的预处理阶段实现字符串连接的操作。
在程序的调试语句中，##常用的方式如下
 `#define DEBUG(fmt, args...) printf(fmt, ##args)` 
	
* 调试宏第一种形式::

	#define DEBUG(fmt, args...)             
    {                                   
		printf("file:%s function: %s line: %d ", __FILE__, __FUNCTION__, __LINE__);
		printf(fmt, ##args);                
    }
	
* 调试宏的第二种定义方式::

	#define DEBUG(fmt, args...)    printf("file:%s function: %s line: %d "fmt, __FILE__, __FUNCTION__, __LINE__, ##args)
	
* 实例::

	#include <stdio.h>

	#define USE_DEBUG  //开启DEBUG宏

	#if USE_DEBUG
        #define DEBUG(fmt, args...) printf("file:%s function: %s line: %d "fmt, __FILE__, __FUNCTION__, __LINE__, ##args)  
	#else
	#define DEBUG(fmt, args...)
	
	int main(int argc, char **argv) {
		char str[]="Hello World";
		DEBUG("%s",str);
		return 0;
	}
	
输出如下::

	Date: Oct  5 2018,File: /code/main.c, Line: 00013: Hello World
	sandbox> exited with status 0
	
* C99编译器标准允许你可以定义可变参数宏(variadic macros)，但不被ANSI/ISO C++ 所正式支持。
 `#define debug(...) printf(__VA_ARGS__)`   

* printf()函数重定向方法

方法一：
1. 使用MicroLIB库，在KEIL-MDK中勾选Use MicroLIB选项
2. 重定向fputc函数

.. code-block:: C

	#include   <stdio.h> 
	int  fputc ( int  ch ,   FILE *  stream ) 
	{    
		//USART_SendData(USART1, (unsigned char) ch);      
		//while (!(USART1->SR & USART_FLAG_TXE));     
		USART_SendChar ( USART1 ,   ( uint8_t ) ch );      
		return  ch ; 
	}
	   
方法二：
半主机模式::

	#pragma   import ( __use_no_semihosting )                               
	struct  __FILE  
	{
		int  handle ;   
	};  

	FILE  __stdout ;            
	_sys_exit ( int  x )   
	{       
		x  =  x ;   
	}  

	int  fputc ( int  ch , FILE   * f )
	{             
		while (( USART1 -> SR & 0X40 )== 0 );     
		
		USART1 -> DR  = ( u8 ) ch ;             
		return  ch ; 
	}