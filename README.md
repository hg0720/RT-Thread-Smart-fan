# 基于 RT-Thread 的智能加湿风扇

## 作品背景

一款基于 RT-Thread 操作系统的自动检测温湿度情况进行风扇转速自动调节，加湿自动启停的桌面风扇。

所用硬件：

* 主控：CH32V307 开发板。
* 温湿度检测：AHT10 温湿度模块。
* 风扇主体：SD7 数字舵机、直流有刷电机、加湿模块。
* 显示屏：陶晶驰智慧串口屏（带触控）。

软件平台：

* RT-Thread Studio   （ 主体开发 ） 
* USART HMI             （ 串口屏开发 ）

## 实现功能

* 手动控制摇头、加湿、加减档、切换模式。
* 连接 WiFi 自动校时。
* 根据温湿度自动加减风速并开关加湿器。

## 整体框架

### 项目框架

本作品以 CH32V307 为主控，通过温湿度传感器 AHT10 采集温湿度信息，再利用 ESP8266 进行 RTC 时间校准。将信息显示到显示屏，也可通过显示屏触摸按钮实现风扇的控制。

![流程总图](https://github.com/hg0720/RT-Thread-Smart-fan/blob/main/assets/%E6%B5%81%E7%A8%8B%E6%80%BB%E5%9B%BE.png)

### 编程思路

将整个系统拆分为多个小任务，可拆分为：温湿度采集任务、时间校对任务、舵机控制任务、电机控制任务、加湿器启停任务等。为各个任务创建一个线程，再利用信号量和邮箱实现各线程的同步和通信。

![软件思路](https://github.com/hg0720/RT-Thread-Smart-fan/blob/main/assets/%E8%BD%AF%E4%BB%B6%E6%80%9D%E8%B7%AF.png)

## RT-Thread 使用情况

* 信号量

  创建了多个信号量，用于触发串口数据接收、电机加减档、舵机摇头、加湿器启停等交互。

* 邮箱

  创建邮箱用于串口数据传输及温湿度信息的传输。

* 线程

  为每个任务创建了一个线程。

## 硬件框架

* PWM 驱动

  用于驱动电机和舵机。

* PIN 设备驱动

  用于控制加湿器启停。

* 串口

  数据传输和人机交互。

## 软件框架

* AHT10 软件包（ 用于读取温湿度数据 ）
* AT DEVICE 软件包（ 用于控制 ESP8266 ）
* netutils 软件包 （ 用于网络时间校准 ）

## 作品图片

![实物1](https://github.com/hg0720/RT-Thread-Smart-fan/blob/main/assets/IMG_20220803_183028.jpg)

![实物2](https://github.com/hg0720/RT-Thread-Smart-fan/blob/main/assets/IMG_20220803_183101.jpg)

## 待改进

* 远程遥控功能。
* 程序优化。

# B站视频链接：https://www.bilibili.com/video/BV1NB4y1t7ri?spm_id_from=333.999.0.0&vd_source=3d603a1fd3341399e9ca2c1017ecad30

