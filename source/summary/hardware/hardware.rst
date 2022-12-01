硬件
===========
这里应当写入 **硬件** 的内容

必要的话，还有目录

- 硬件的通俗介绍
- 硬件的全部资料

MCU
-------------------
RBZ V1.0采用 **ESP32-WROOM-32E** 作为MCU。该模组具备 **WI-FI、蓝牙通信功能** ，内置ESP32-DOWN-V3芯片、ESP32-D0WDR2-V3芯片、Xtensa⑧双核32位LX6微处理器，支持高达240 MHz的时钟频率，拥有448KB ROM、520KB SRAM、16 KB RTC SRAM，采用PCB板载天线，配置了4MB SPI flash。

- WI-FI支持A-MPDU和A-MSDU聚合、支持0.4μS保护间隔，工作信道中心频率范围: 2412 ~ 2484 MHz
- 蓝牙支持v4.2完整标准（包含传统蓝牙BR/EDR和低功耗蓝牙BLE），支持标准Class-1、Class-2 和Class-3，支持自适应跳频AFH、CVSD和SBC音频编解码算法
- 模组接口: SD卡、UART、SPI、SDIO、I2C、LEDPWM、电机PWM、I2S.、IR、脉冲计数器、GPIO、电容式触摸传感器、ADC、DAC。
- 40MHz晶振
- 板载PCB天线
- 8MB SPI flash
- 工作电压/供电电压: 3.0~3.6 V 

.. image:: assets/MCU_ESP32.jpg

HUB
------------------
HUB为多端口转发器，支持I2C、I2S通讯协议。

.. image:: assets/HUB.jpg

POWER
------------------
RBZ V1.0采用AMS1117-3.3作为降压稳压模块给芯片供电。AMS1117是一个正向低压降稳压器，在1A电流下压降为1.2V。

- 输出电压：3.267~3.333V
- 线路调整（最大）：10mV（4.75V《=VIN《=12V）
- 负载调节（最大）：15mV
- 电压差（最大）：1.3V
- 电流限制：900~1500mA
- 静态电流（最大）：10mA
- 纹波抑制（最小）：60dB

其电路如下图所示。

.. image:: assets/POWER.jpg
    
图中C6为输入电容，防止断电后电压倒置。C7为输出滤波电容，用于抑制自激振荡。

USB TO UART
------------------
RBZ V1.0采用CH340C转接芯片实现USB转串口。在串口方式下，CH340C提供常用的MODEM 联络信号，用于为计算机扩展异步串口，或者将普通的串口设备直接升级到USB总线。

- 全速USB设备接口，兼容USB V2.0。
- 硬件全双工串口，内置收发缓冲区，支持通讯波特率50bps~2Mbps。
- 支持常用的MODEM联络信号RTS、DTR、DCD、RI、DSR、CTS。
- 支持5V电源电压和3.3V电源电压。
- 内置时钟，无需外部晶振。
- 提供SOP-16、SOP-8和SSOP-20以及ESSOP-10、MSOP-10无铅封装，兼容RoHS。

.. image:: assets/USB_TO_UART.jpg
 
POWER
------------------
RBZ V1.0采用IN5819HW-7-F作为表面整流二极管，适用于低电压高频率逆变器.整流二极管具有高电流能力与低正向压差。AMS1117-3.3_C347256在前文POWER章节已描述，此处不再赘述。

- 最大正向电压降：750mV
- 最大连续正向电流：10A
- 峰值反向重复电压：40V
- 峰值非重复正向浪涌电流：25V

.. image:: assets/POWER2.jpg

   
POWER LED
------------------
POWER LED为上电指示灯。当单片机上电时，LED灯亮。

.. image:: assets/POWER_LED.jpg

AUTO PROGRAM
------------------

.. image:: assets/AUTO_PROGRAM.jpg

USB
------------------

RBZ V1.0采用TYPE-C-2.0-16PIN--SMT-3作为USB连接器。
.. image:: assets/USB.jpg



LED
------------------
当IO2输入高电平，LED发光。

.. image:: assets/LED.jpg

KEY
------------------
KEY为按键模块。

.. image:: assets/KEY.jpg

IO
---------------
扩展IO

UART LED
---------------
串口LED

电机？？？
---------------
