# VEX_communication
Enable third-party communication with VEX Brain V5.  

# Tutorial: Linking VEX Brain V5 with Arduino (or Raspberry Pi, esp32 etc.)(English&Chinese)
# 教程：把VEX V5主控与Arduino（或者树莓派，esp32等）连接通信（中英双语）  
### *All of the files can be found in the link below
### *所有文件可以在下方链接找到
https://github.com/Maotechh/VEX_communication  
⭐️Star if you like it  
⭐️如果你喜欢请给个Star  

Hey everyone! Today I'll show you how to handle communication between VEX Brain V5 and Arduino(as example). First we need to know the logic of their communication.  We can know the pinout of smart ports from [here](https://www.vexforum.com/t/smart-ports-power-wire/68061/3), so we can find out a simple process:  
大家好！今天我将手把手教你们如何将V5主控和Arduino通信。首先我们需要知道他们通信的逻辑。[这里](https://www.vexforum.com/t/smart-ports-power-wire/68061/3)已经指出了vex主控智能端口的引脚定义，因此我们能够知道一个简单的流程：  
![pinout](https://www.vexforum.com/uploads/default/original/3X/9/1/91e8a35756d1b1d22f98e72cd5b3a9c17802480a.jpeg)  
Black: RS485-A  
Red: RS485-B  
Green: GND  
Yellow: Power(12V)  
## Hardware 硬件
First of all, we need to buy an RS485 to TTL module in order to change the A and B lines of RS485 represented by the black and red lines of VEX to TTL level, and connect the A and B lines and GND on the right side.  
首先我们需要购买RS485转TTL模块，将VEX的黑和红线所代表的RS485的A和B线变为TTL电平，把A和B线还有GND在右侧接上。  
![rs485-to-ttl](https://img.alicdn.com/imgextra/i3/738263294/O1CN01Tjb6ot1aChAvA4TUD_!!738263294.jpg)
Then buy a 12V to 5V buck module to power the Arduino. Connect GND and VIN to VEX's GND and Power, connect GND and Vout to Arduino's GND and VCC (5V)  
然后购买12V转5V降压模块，来给Arduino供电。将GND和VIN连接VEX的GND和Power，将GND和Vout连接Arduino的GND和VCC（5V）  
![12v-to-5v](https://gw.alicdn.com/bao/uploaded/i3/2207691322/O1CN01x2W8u91LdWPSAe4E5_!!2207691322.jpg_Q75.jpg_.webp)
I made a pcb board to allow the VEX to plug into a breadboard and bring out the pins.  
我制作了一个pcb板来让VEX能够插在面包板上，引出各引脚。   
![pcb](https://user-images.githubusercontent.com/109655023/255457680-854f7c40-2a88-4538-817a-d9a4ef31fd5d.png)  
All you have to do is as follows:  
你所有要做的如下：  
![connect](https://user-images.githubusercontent.com/109655023/255461725-1523d2a2-a6aa-4c24-b997-19af1b3c4ca5.png)  

## Software 软件
Sending information through the serial port on the Arduino side requires this  
在Arduino端通过串口发送信息需要这样  
```arduino
#define BAUDRATE 115200
//Baudrate, make sure it's the same with VEX side. 
//波特率，确认比特率大小与VEX端相同
void setup() {
    Serial.begin(1200);
}
void loop() {
    Serial.write('What_you_want_to_send')
}
```
Receiving information through the serial port on the VEX side requires this  
在VEX端通过串口接收信息需要这样  
```c++
//robot-config.cpp
motor Arduino = motor(PORT20, ratio18_1, false);

//main.cpp
#define BAUDRATE 115200
int main() {

  // Initializing Robot Configuration. DO NOT REMOVE!
  vexcodeInit();

  vexGenericSerialEnable(Arduino.index(), 0);
  vexGenericSerialBaudrate(Arduino.index(), BAUDRATE);
}
```
Here are all the functions for using the VEX as a generic device.  
这是将VEX用作通用设备的所有函数。  
```h
//v5_apiuser.h
// Generic serial port comms to any device
void                vexGenericSerialEnable( uint32_t index, int32_t options );
void                vexGenericSerialBaudrate( uint32_t index, int32_t baudrate );
int32_t             vexGenericSerialWriteChar( uint32_t index, uint8_t c );
int32_t             vexGenericSerialWriteFree( uint32_t index );
int32_t             vexGenericSerialTransmit( uint32_t index, uint8_t *buffer, int32_t length );
int32_t             vexGenericSerialReadChar( uint32_t index );
int32_t             vexGenericSerialPeekChar( uint32_t index );
int32_t             vexGenericSerialReceiveAvail( uint32_t index );
int32_t             vexGenericSerialReceive( uint32_t index, uint8_t *buffer, int32_t length );
void                vexGenericSerialFlush( uint32_t index );
```
### *All of the files can be found in the link below
### *所有文件可以在下方链接找到
https://github.com/Maotechh/VEX_communication  
⭐️Star if you like it  
⭐️如果你喜欢请给个Star  
# Wish you good luck !
# 祝你好运！
