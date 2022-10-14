.. _doc_tutorial_basic_03_marquee:

逻辑控制——跑马灯
==================================================

我们将会编写程序，
控制多个LED/灯带依次亮灭，
交替循环。

这一小节将系统性地介绍：

1. Arduino IDE中数组的使用。
2. for循环。

数组
~~~~~~~~~~~~~~~~~~~~~
数组（Array）是一个固定长度的储存相同数据类型的数据结构。
数组中的元素被储存在一段连续的内存空间中。
在程序中使用数组可以更加便捷的对一系列有相同特征的变量进行索引。
以下是声明数组的一种方式
::

   int array[] = {};//声明一个数组

for循环
~~~~~~~~~~~~~~~~~~~~~
for循环是一种循环语句，语法如下
::

   for(i = 0;i <= length; i++)
   {     
      //循环内容
   }

i为计数器，length为i在for循环中的最大值。
循环语句极大便利了程序的编写，但由于计数器i的介入，其算法复杂度会提高。

编写代码
~~~~~~~~~~~~~~~~~~~~~

首先声明LED的GPIO引脚和循环中计数器。

::

   const int LED_PINS[] = {26,25,33,32};
   int i,j=3;


然后来设置LED所属的引脚为 **输出模式**。

::

   pinMode(LED_PINS[0], OUTPUT);
   pinMode(LED_PINS[1], OUTPUT);
   pinMode(LED_PINS[2], OUTPUT);
   pinMode(LED_PINS[3], OUTPUT);

接着，
我们在loop()函数中，
添加代码来 **实现LED引脚电平的依次变化**，
从而使LED灯实现依次闪烁。

::

   or(i = 0;i <= 3;i++)
    {
      digitalWrite(LED_PINS[i], HIGH);
      digitalWrite(LED_PINS[j], LOW);
      j = i;
      delay(500);
    }

在同一个循环中，j会保持i在上一循环的值，
以实现前一灯的熄灭与后一灯的点亮。

整个程序最后便是：

.. code-block:: arduino
   :linenos:

   const int LED_PINS[] = {26,25,33,32};
   int i,j=3;
   void setup() {
      pinMode(LED_PINS[0], OUTPUT);
      pinMode(LED_PINS[1], OUTPUT);
      pinMode(LED_PINS[2], OUTPUT);
      pinMode(LED_PINS[3], OUTPUT);
   }

   void loop() {
      for(i = 0;i <= 3;i++)
      {
         digitalWrite(LED_PINS[i], HIGH);
         digitalWrite(LED_PINS[j], LOW);
         j = i;
         delay(500);
      }
   }  
