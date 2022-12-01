.. _doc_tutorial_basic_05_buzzer_tone:

.. highlight:: arduino

DAC输出——蜂鸣器响度控制
==================================================

我们将会编写程序，
控制蜂鸣器响度逐渐变大再变小，
交替循环。

这一小节将系统性地介绍：

1. DAC的概念（数模转换器）
2. 蜂鸣器响度控制的实现方式——PWM。
3. ledcWrite（）函数的使用。

前言
~~~~~~~~~~~~

BUZZER
----------------

我们使用的是无源蜂鸣器。
这里的“源”指的不是电源，而是有无震荡源。
有源蜂鸣器内有震荡电路，因此输入直流信号即可驱动振膜震动发声。
而无源蜂鸣器需要输入PWM信号，以驱动振膜。
在RBZBoard上没有现成的蜂鸣器，因此需要外接，我们选择正极接入26端口。


方波
----------------

方波是一种只有0（低电平）和1（高电平）的一种波。
由于计算机电路内部也只有0和1两种状态。
方波也是计算机中最为重要的波形之一。

DAC(模数转换)
~~~~~~~~~~~~~~~~~~~~~

数模转换器指的是将数字量转变为模拟量的器件
模拟量通常指的是连续信号，是一种在时间和幅度上都连续的一种信号。

数字量是一种离散信号，通常是通过二进制进行表达。
例如三位二进制数（011）可以转换为十进制数3。
这也是DAC在计算机中实现的过程。



PWM
~~~~~~~~~~~~~~~~~~~~~

PWM调制可以通过控制恒定频率的方波的占空比来实现功率变化。
::

   占空比：δ = t/T. //t为在一个周期内高电平的时长，T为周期
   功率：P = U^2/R. //U为电压，R为电阻
   设外围电路阻值不变，即P ∝ U^2. 
   设Um为高电平电压，可得
   P = (Um*δ)^2/R.

由上式可知，占空比δ越大，输出功率也就越大。
即可控制蜂鸣器的响度。



ledcWrite（）函数的使用
~~~~~~~~~~~~~~~~~~~~~

首先ledcWrite（）函数的初始化。

::

   ledcSetup(channel,freq,resolution);   //设置通道，频率及分辨率

共有16个通道（channel），为0-15。
频率（freq）指的是PWM波的频率。
分辨率取值为0-20，其模拟量范围为0-2^resolution-1。
分辨率指的是将输出电压0-3.3V平均分为2^resolution份。

之后是将通道与GPIO相连接
::

   ledcAttachPin(GPIO,channel);           //将通道0和gpio_pin连接起来

最后通过ledcWrite（）函数即可控制输出的PWM占空比。

::

   ledcWrite(channel,i); //i为输出的模拟量，为十进制数。

i的变化可以通过for循环来改变。
整个程序最后便是：

.. code-block:: arduino
   :linenos:

   int BUZZER = 26;
   int freq = 2000;    //设置PWM波的频率
   int channel = 0;    //设置通道,共16个通道，0~15
   int resolution = 10; //分辨率，取值0~20 duty的最大值为 2^resolution-1
   void setup() {
      ledcSetup(channel,freq,resolution);   //设置通道0
      ledcAttachPin(BUZZER,channel);           //将通道0和gpio_pin连接起来
   }

   void loop() {
      // led逐渐变暗
      for (int  i = 0; i < 1023; i=i+5)
      {
         ledcWrite(channel,i);
         delay(5);
      }
      // led逐渐变亮
      for (int  i = 1023; i >= 0 ; i=i-5)
      {
         ledcWrite(channel,i);
         delay(5);
      }
   }

最后将程序上传至单片机即可运行。