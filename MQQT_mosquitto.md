

## 一.简单介绍

  1.[MQTT](http://mqtt.org/)是IBM开发的一个即时通讯协议。MQTT是面向M2M和物联网的连接协议，采用轻量级发布和订阅消息传输机制。 

【MQTT协议特点】——[MQTT](http://mqtt.org/)协议借助消息推送功能，可以更好地实现远程控制。
【MQTT协议角色】——在MQTT协议中包括发布者，代理器（服务器）和订阅者。
【MQTT协议消息】——MQTT中的消息可理解为发布者和订阅者交换的内容（负载），这些消息包含具体的内容，可以被订阅者使用。
【MQTT协议主题】——MQTT中的主题可理解为相同类型或相似类型的消息

发布者——也就是产生数据者，实际系统中可能就是采集某个信号的小设备，比如一个采集温湿度的设备，但是这个设备是可以连接到网络的，所以它可以向代理（服务器）发布消息。
代理——也就是数据中转服务器，它接收发布者发布的消息数据，比如说具体的温湿度值，然后接收到这些数据之后可以推送给订阅者。
订阅者——也就是接收发布者发布的消息接收者，比如是一部手机或者电脑之类的，它能实时显示发布者通过代理发送过来的数据。
举个简单例子：我做了一个小设备，能采集环境温湿度数据，并具备以太网功能，也就是说它采集到的数据可以发送到以太网，当然我也有一台服务器，这个服务器可以是真实的服务器，也可以用树莓派这样的设备来充当服务器，它主要负责数据的中转，然后我有一个能上网的手机（别告诉我你现在的手机不能上网），这个手机上有我用MQTT协议写的一个软件。然后最终我能实现的目的就是，传感器采集到的温湿度，通过以太网将数据发送到代理（服务器），然后代理（服务器）将该数据推送到我的手机上来

  2.[Mosquitto](http://mosquitto.org/)是一款实现了 MQTT v3.1 协议的开源消息代理软件，提供轻量级的，支持发布/订阅的的消息推送模式，使设备对设备之间的短消息通信简单易用。更多资料请访问：MQTT官网：[点这里](http://mqtt.org/)



## 二. [Mosquitto](http://mosquitto.org/) 安装

### from a terminal run the following commands:

`$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

### Install mosquitto from brew:

`$ brew install mosquitto`

## 三. mosquitto服务启动命令

停止服务  brew services stop mosquitto

启动服务  brew services start mosquitto

## 四.配置服务器地址和端口号

mosquitto 服务器的配置环境文件路径 /usr/local/etc/mosquitto/mosquitto.conf

打开mosquitto.conf文件来配置你自己的IP和端口号

 #bind_address ip-address/host name

bind_address 127.0.0.1(在本地进行，若是远程服务器要写对应的IP地址)

 #port to use for the default listener.

port 1883 

## 五、**简单测试**

​    一个完整的MQTT示例包括一个代理器，一个发布者和一个订阅者。在本例中，发布者、代理和订阅者均为localhsot，但是在实际的情况下三种并不是同一个设备，在mosquitto中可通过-h(--host)设置主机名称(hostname)。为了实现这个简单的测试案例，需要在MAC中打开三个控制台，分别代表代理服务器、发布者和订阅者。

测试分为以下几个步骤：

【1】启动服务mosquitto。

 打开一个终端，brew services start mosquitto

【2】订阅者通过mosquitto_sub订阅指定主题的消息。

  打开另外一个终端，mosquitto_sub -h 127.0.0.1 -t sensor

​    【-t】指定主题，此处为sensor，sensor是用户自己随便取的一个名字，主要用来区分每个消息属于哪一个主题。
​    【-v】打印更多的调试信息
​    【-h】指定代理器的IP地址或者域名

【3】发布者通过mosquitto_pub发布指定主题的消息。

  打开第三个终端，$ mosquitto_pub -h 127.0.0.1 -t sensor -m "25"

​    【-t】指定主题
​    【-m】指定消息内容
​    【-h】指定代理器的IP地址或者域名

【4】代理服务器把该主题的消息推送到订阅者。

  在第二个终端窗口会显示“25”

http://blog.csdn.net/xukai871105/article/details/39255089