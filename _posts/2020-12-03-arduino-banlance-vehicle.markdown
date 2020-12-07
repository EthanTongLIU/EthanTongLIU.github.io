---
layout:       post
title:        "基于Arduino的智能平衡小车"
subtitle:     "Arduino UNO 主控 + 外设的两轮智能平衡小车制作全过程记录"
date:         2020-12-03 20:04:00
author:       "LiuTong"
header-mask:  0.2
catalog:      true
multilingual: false
tags:
    - Arduino
    - 嵌入式
---

## 零件列表

|  零件   | 型号 | 作用 | 
|  ----  | ----  | ----  | 
| Arduino | UNO | 主控 |
| 电机驱动板 | L298N | 驱动电机 |
| DC-DC降压模块 | LM2596带数显 | 提供5V稳压 |
| 三轴陀螺仪加速度计 | MPU6050 | 姿态解算 |
| 0.96寸OLED屏 | L-308 4pin I2C 128x64 | 显示 |  
| 直流减速电机带霍尔编码测速 | GB37-520 | 动力 |
| 18650锂电池组 | 12V 3600mAh | 供电 |
| 超声波测距模块 | HC-SR04 | 测距 |

## 电路连接

|  UNO  | L298N | 18650(12V) | MPU6050 | OLED | LM2596 | 电机 |
|  ---- | ----  | ---------  | ------- | ---- | ------ | ---- |
|  GND  |  GND  |     GND    |   GND   |  GND |  VIN-  |      |
|       |  +12V | 12V(给L298N和LM2596供电) |  |  | VIN+ |
|  | +5V(给电机码盘供电) |  |  |  |  | VCC |
| 3(PWM) | ENA |  |  |  |
| 0 | IN1 |  |  |  |
| 1 | IN2 |  |  |  |
| 2 | IN3 |  |  |  |
| 4 | IN4 |  |  |  |
| 6(PWM) | ENB |  |  |  |
|  | IN2 |  |  |  |
| 5V(给MPU6050供电) |  |  | VCC |
| A4 |  |  | SDA | SDA |
| A5 |  |  | SCL | SCL |
| 3.3V(给OLED供电) |  |  |  | VCC |

## 电机测试

#### 电机控制

###### L298N电机驱动

###### 控制代码

```cpp
/*
 * 电机控制测试程序
 * 2020/12/07
 * 作者：刘通
 * ENA、ENB接入Arduino PWM接口用于电机调速。
 * IN1、IN2、IN3、IN4接口，分别用来控制两路直流电机前进、后退、转向以及刹车，
 * 只需接入Arduino的数字接口即可。
 * 并且采用串口通信指令控制电机。
*/

//motor A
int IN1 = 7;   // IN1 connected to pin 0
int IN2 = 8;   // 
int ENA = 6;   // PWM调速

//motor B
int IN3 = 2;
int IN4 = 4;
int ENB = 5;
 
unsigned long time = 2000;  //delay time
int value = 240;   // the duty cycle 控制电机转速 0-255
int state = 0; // 串口数据

// 前进
void goForward(){
    analogWrite(ENA, value); // 输入模拟值设定速度
    analogWrite(ENB, value);
    digitalWrite(IN1, 1); // 使Motor A顺时针转
    digitalWrite(IN2, 0);
    digitalWrite(IN3, 1); // 使Motor B逆时针转
    digitalWrite(IN4, 0);
}

// 后退
void goBack(){
    analogWrite(ENA, value); 
    analogWrite(ENB, value);
    digitalWrite(IN1, 0); 
    digitalWrite(IN2, 1);
    digitalWrite(IN3, 0); 
    digitalWrite(IN4, 1);
}

// 右转
void turnRight(){
    analogWrite(ENA, value/2); // 输入模拟值设定速度
    analogWrite(ENB, value/2);
    digitalWrite(IN1, HIGH); 
    digitalWrite(IN2, LOW);
    digitalWrite(IN3, LOW); 
    digitalWrite(IN4, HIGH);  
}

// 左转
void turnLeft(){
    analogWrite(ENA, value/2); // 输入模拟值设定速度
    analogWrite(ENB, value/2);
    digitalWrite(IN1, LOW); 
    digitalWrite(IN2, HIGH);
    digitalWrite(IN3, HIGH); 
    digitalWrite(IN4, LOW);    
}

// 刹车
void brake(){
    digitalWrite(IN1, HIGH); 
    digitalWrite(IN2, HIGH);
    digitalWrite(IN3, HIGH); 
    digitalWrite(IN4, HIGH);  
}

void setup(){
    //设置GPIO口为输出
    pinMode(IN1, OUTPUT);
    pinMode(IN2, OUTPUT);
    //pinMode(ENA, OUTPUT);
    pinMode(IN3, OUTPUT);
    pinMode(IN4, OUTPUT);
    //pinMode(ENB, OUTPUT);                                                                                             

    // 串口设置
    Serial.begin(9600); //设置串口波特率9600
}
 
void loop(){
    if (Serial.available() > 0){ //串口接收到数据
        state = Serial.read();   //获取串口接收到的数据
        if (state == 'F'){
            Serial.println("前进");
            goForward();
        }
        if (state == 'B'){
            Serial.println("后退");
            goBack();
        }
        if (state == 'R'){
            Serial.println("右转");
            turnRight();
        }
        if (state == 'L'){
            Serial.println("左转");
            turnLeft(); 
        }
        if (state == 'S'){
            Serial.println("刹车");
            brake(); 
        }
    }
    //delay(time);
}
```

#### 电机测速

###### 测速原理

###### 测速代码

一、两台电机单相二倍频测速

```cpp
/**
* 描述: 单台电机测速
* 日期：2020/12/06
* 作者：刘通
*/

#include <MsTimer2.h> //需要安装

//MotorA
const int IN1 = 7;
const int IN2 = 8;
const int ENA = 6;

//MotorB
const int IN3 = 2;
const int IN4 = 4;
const int ENB = 5;

const int MotorAM = 10;  // 定义检测A电机一相脉冲的引脚
const int MotorBM = 12;  // 定义检测B电机一相脉冲的引脚

int times = 0, newtime = 0, d_time = 100;  //时间，最新的时间，时间间隔，单位：ms
int valA = 0, valB = 0, flagA = 0, flagB = 0;  //变量valA和valB用于计算脉冲数

void setup(){
    Serial.begin(9600);
  
    // GPIO为输出
    pinMode(IN1,OUTPUT);
    pinMode(IN2,OUTPUT);
    pinMode(ENA,OUTPUT);
    pinMode(IN3,OUTPUT);
    pinMode(IN4,OUTPUT);
    pinMode(ENB,OUTPUT);
  
    //GPIO为输入
    pinMode(MotorAM,INPUT);
    pinMode(MotorBM,INPUT);
  
    MsTimer2::set(d_time, inter); // d_time触发一次中断，调用函数inter()
    MsTimer2::start();    //开启中断
}

void loop(){
    //两电机都正转
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, LOW);
    digitalWrite(IN3, HIGH);
    digitalWrite(IN4, LOW);
    analogWrite(ENA, 255);    //写入PWM值0~255（速度）
    analogWrite(ENB ,255);
  
    // 左侧电机A相二倍频测速，一个脉冲计数2次（高电平1次+低电平1次）
    if(digitalRead(MotorAM) == HIGH && flagA == 0){
        valA++;
        flagA = 1;
    }
    if(digitalRead(MotorAM) == LOW && flagA == 1){
        valA++;
        flagA = 0;
    }
  
    // 右侧电机A相二倍频测速，一个脉冲计数2次（高电平1次+低电平1次）
    if(digitalRead(MotorBM)==HIGH && flagB == 0){
        valB++;
        flagB = 1;
    }
    if(digitalRead(MotorBM) == LOW && flagB == 1){
        valB++;
        flagB = 0;
    }
}

//中断函数
void inter(){
    sei();    //允许全局中断
    Serial.print("MotorA = "); //在串口监视器上打印出脉冲值
    Serial.print(valA/(2*0.33*d_time));
    Serial.println("r/s");
    Serial.print("MotorB = ");
    Serial.print(valB/(2*0.33*d_time));
    Serial.println("r/s");
    valA = valB = 0; //清0
}
```

二、单台电机两相四倍频测速

```cpp
/**
* 日期: 2017/02/28
* 原作者: 呆萌阿宝
* 修改日期：2020/12/06
* 作者：刘通
* 描述: 单台电机测速
*/

const int d_time = 100; //设定单位时间，单位：ms
int flagA = 0; //标志位设定
int flagB = 0;

int AM1 = 10; //A相输入引脚
int BM1 = 11; //B相输入引脚

int IN1 = 7; //IN1 接 pin7
int IN2 = 8; //IN2 接 pin8
int ENA = 6; //使能调速

int valA = 0; //A相脉冲数
int valB = 0; //B相脉冲数

double n; //转速，单位：r（转）/s

unsigned long times;
unsigned long newtime; //时间变量

// 前进
void go(int speed)
{
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, LOW);
    analogWrite(ENA, speed);
}

// 后退
void back(int speed)
{
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, HIGH);
    analogWrite(ENA, speed);
}

// 刹车
void stay()
{
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, HIGH);
}

void setup()
{
    Serial.begin(9600); //串口初始化，波特率9600

    pinMode(IN1,OUTPUT); //GPIO为输出
    pinMode(IN2,OUTPUT); //GPIO为输出
    pinMode(ENA,OUTPUT); //GPIO为输出

    pinMode(AM1,INPUT); //GPIO 为输入
    pinMode(BM1,INPUT); //GPIO 为输入
}

void loop()
{
    go(255);
    newtime = times = millis();

    while((newtime-times) < d_time)
    {
        if(digitalRead(AM1) == HIGH && flagA == 0)
        {
            valA++;
            flagA = 1;
        }
        if(digitalRead(AM1) == LOW && flagA == 1)
        {
            valA++;
            flagA = 0;
        }
        if(digitalRead(BM1) == HIGH && flagB == 0)
        {
            valB++;
            flagB = 1;
        }
        if(digitalRead(BM1) == LOW && flagB == 1)
        {
            valB++;
            flagB = 0;
        }
        newtime = millis();
    }//计算AB两相的脉冲数

    n = (valA+valB)/(1.32*d_time); //计算转速，减速比30，一圈11个脉冲，减速后一圈330个脉冲，4个相330x4=1320个脉冲
    Serial.print(n);
    Serial.println("r/s"); //输出转速数值
    valA = valB = 0; //清零储存脉冲数的变量
}
```

## 算法

#### 位置环+速度环

#### PID

## 调试

## 外设制作

## 版本更迭

- 蓝牙/WiFi无线通信模块，实现远程控制

- OLED屏幕，实现文字或图案显示

- 超声波测距模块，实现自动避障测距等

- 毫米波雷达模块，实现远距离精确测距等

- 激光雷达扫描模块，实现自动扫描周边环境采集点云数据等
