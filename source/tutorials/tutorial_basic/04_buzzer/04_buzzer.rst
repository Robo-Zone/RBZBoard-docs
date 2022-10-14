.. _doc_tutorial_basic_04_buzzer:

DAC输出——外接蜂鸣器演奏小星星
============================

我们将会编写程序，
通过外接蜂鸣器，
演奏小星星。

这一小节将系统性地介绍：

1. DAC输出。
2. tone()函数的使用。

前言
~~~~~~~~~~~~~~~~~~~

蜂鸣器
------------------

蜂鸣器种类繁多，主要分为有源蜂鸣器和无源蜂鸣器。蜂鸣器有正、负极性之分，采用直流电压供电，广泛用于打印机、报警器等电子设备中作为其发声元件。

有源蜂鸣器发声原理：
直流电源输入经过振荡系统的放大取样电路在谐振装置作用下产生声音信号。
无源蜂鸣器发生原理：方波信号输入谐振装置转换为声音信号输出。
RBZBoard上没有蜂鸣器，需要外接，蜂鸣器引脚 **正极接pin4，负极接GND**。


tone()函数
---------------------

tone()函数可以产生 **固定频率的PWM信号** 来驱动扬声器发声。 发声时间长度和声调都可以通过参数控制。

定义发声时间长度有两种方法:
1. 通过tone()函数的参数来定义发声时长;
2. 使用noTone()函数来停止发声。

.. note::
    如果在使用tone()函数时没有定义发声时间长度，那么除非通过noTone()函数来停止声音，否则Arduino将会一直通过tone()函数产生声音信号。

    如果想要使用不同的引脚产生不同的声音音调，每一次更换发声引脚以前都要使用noTone函数停止上一个引脚发声。


编写代码
~~~~~~~~~~~~~~~~~~~~~

我们需要定义不同音对应的频率，在最外层添加代码：

::
    // 定义不同音符对应频率
    #define NOTE_D0 -1
    #define NOTE_D1 294
    #define NOTE_D2 330
    #define NOTE_D3 350
    #define NOTE_D4 393
    #define NOTE_D5 441
    #define NOTE_D6 495
    #define NOTE_D7 556 
    
我们需要定义曲子的节拍，在最外层添加代码：
::
    #define WHOLE 1 // 定义全音符
    #define HALF 0.5 //定义二分音符
    

我们需要定义一个数组，表示整首曲子的音符部分：
::
    int tune[] =
    {
    NOTE_D1, NOTE_D0, NOTE_D1, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D5, NOTE_D0, // Twinkle, twinkle, little star
    NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D1, NOTE_D0, // How I wonder what you are!
    NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, // Up above the world so high,
    NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, // Like a diamond in the sky.
    NOTE_D1, NOTE_D0, NOTE_D1, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D5, NOTE_D0, //Twinkle, twinkle, little star
    NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D1, NOTE_D0  //How I wonder what you are!
    }

我们需要定义一个数组，表示整首曲子的节拍（音符持续时间）：

::
    loat duration[] =
    {
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5,
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5,
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5, 
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5, 
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5,
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5
    }


我们需要定义一个变量用来表示多少音符以及蜂鸣器所接引脚：

::
    int length; // 定义一个变量用来表示共有多少个音符
    int tonePin = 4; // 蜂鸣器接pin4

然后，
我们在setup初始化函数中，
调用pinMode()函数，
来设置蜂鸣器所接4号GPIO引脚为 **输出模式**；
调用sizeof()函数，
来查出数组里有多少个音符。

::
    pinMode(tonePin, OUTPUT); // 设置蜂鸣器的pin为输出模式
    length = sizeof(tune) / sizeof(tune[0]); // 查出数组里有多少个音符

接着，
我们在loop()函数中，
添加代码来循环播放曲子。

::
    for (int x = 0; x < length; x++) // 循环音符的次数
    {
    tone(tonePin, tune[x]); // 依次播放tune数组元素（即每个音符）
    delay(400 * duration[x]); // 每个音符持续的时间duration，400是调整时间的越大，曲子速度越慢，越小曲子速度越快
    noTone(tonePin);// 停止当前音符，进入下一音符
     }
    delay(5000);// 等待5秒后，循环重新开始


整个程序最后便是：
.. code-block:: arduino
    :linenos:

    // 定义不同音符对应频率
    #define NOTE_D0 -1
    #define NOTE_D1 294
    #define NOTE_D2 330
    #define NOTE_D3 350
    #define NOTE_D4 393
    #define NOTE_D5 441
    #define NOTE_D6 495
    #define NOTE_D7 556
 
    #define WHOLE 1 // 定义全音符
    #define HALF 0.5 // 定义二分音符

    // 整首曲子的音符部分
    int tune[] =
    {
    NOTE_D1, NOTE_D0, NOTE_D1, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D5, NOTE_D0, // Twinkle, twinkle, little star
    NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D1, NOTE_D0, // How I wonder what you are!
    NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, // Up above the world so high,
    NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, // Like a diamond in the sky.
    NOTE_D1, NOTE_D0, NOTE_D1, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D5, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D6, NOTE_D0, NOTE_D5, NOTE_D0, //Twinkle, twinkle, little star
    NOTE_D4, NOTE_D0, NOTE_D4, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D3, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D2, NOTE_D0, NOTE_D1, NOTE_D0  //How I wonder what you are!
    }

    // 整首曲子的节拍
    loat duration[] =
    {
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5,
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5,
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5, 
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5, 
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5,
    0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 1, 0.5
    }

    int length; // 定义一个变量用来表示共有多少个音符
    int tonePin = 4; // 蜂鸣器接pin4

    void setup()
    {
    pinMode(tonePin, OUTPUT); // 设置蜂鸣器的pin为输出模式
    length = sizeof(tune) / sizeof(tune[0]); // 查出数组里有多少个音符
    }

    void loop()
    {
        for (int x = 0; x < length; x++) // 循环音符的次数
        {
        tone(tonePin, tune[x]); // 依次播放tune数组元素（即每个音符）
        delay(400 * duration[x]); // 每个音符持续的时间duration，400是调整时间的越大，曲子速度越慢，越小曲子速度越快
        noTone(tonePin);// 停止当前音符，进入下一音符
        }
    delay(5000);// 等待5秒后，循环重新开始
    }



上传单片机
~~~~~~~~~~~~~~~~~~~~~

 