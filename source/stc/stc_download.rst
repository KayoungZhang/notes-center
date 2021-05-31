
.. 标题文字下的符号长度都要大于标题长度

STC单片机纯软件实现程序自动下载功能操作指南
=================================================

原理：
------------------

STC单片机下载程序一般需要冷启动，即需要先点击下载软件上的下载按钮，
然后断电一次，才可以成功下载。在STC官方下载软件（STC-ISP）中，点击下载按钮后，
从电脑端串口是一直在发送数据的，一般是0x7F，也可以是用户自定义，
同时根据STC12C5A60S2芯片手册，在软件复位一章中，可以根据设置IAP_CONTR寄存器（STC89是ISP_CONTR寄存器）
就可以实现不断电复位，从而进入ISP下载区进行程序的下载，具体操作如下：

.. image :: pic/01.png

- 方法一：

.. code-block :: C
	
	sbit AutoDownload = P3^0; 
	int main(void)
	{
		while(1)
		{
			if(AutoDownload == 0)   //STC软件下载提示信号检测
			{
				IAP_CONTR = 0x60;  //从STC的ISP区开始运行程序的软件复位设置
			}
			else
			{
				主循环程序
			}
		}
		return (0);
	}
..


- 方法二：

.. code-block :: C

	unsigned char uCount = 0;     //接收数据个数

	void uart1_init(void) // 9600bps@11.0592MHz
	{
		PCON &= 0x7F;		//波特率不倍速
		SCON = 0x50;		//8位数据,可变波特率
		AUXR |= 0x40;		//定时器1时钟为Fosc,即1T
		AUXR &= 0xFE;		//串口1选择定时器1为波特率发生器
		TMOD &= 0x0F;		//清除定时器1模式位
		TMOD |= 0x20;		//设定定时器1为8位自动重装方式
		TL1 = 0xDC;		//设定定时初值
		TH1 = 0xDC;		//设定定时器重装值
		ET1 = 0;		//禁止定时器1中断
		ES = 1;		//启动串口中断
		TR1 = 1;		//启动定时器1
		EA = 1;
	}

	Void  UART1_Handler()  interrupt  4
	{
		if (RI)
		{
			RI = 0;             //Clear receive interrupt flag
			if(SBUF == 0x7f)
			{
				uCount++;	
				if(uCount == 10)   //自定义区连续发送10个0x7F，即可实现自动热复位下载
				{
					uCount = 0;	
					IAP_CONTR = 0x60;    //软复位到系统ISP监控区
				}
			}
			else
			{
				uCount = 0;
			}
		}
	} 
..

在main函数中，对uart1进行初始化后，然后在STC-ISP软件自定义区进行设置，如下：

.. image :: pic/02.png

第一次下载，还需要冷启动下载，之后就不需要了。





















