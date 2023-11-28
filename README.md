# Sip_Qt_client
# 这是一款VOIP的客户端软件，具有高稳定性，高音质的优点

# 该软件基于Pjsip协议栈，采用QT开发框架，C/C++语言开发。

## 1.什么是SIP？

### **SIP协议的功能**

1. **用户定位**：SIP协议中被叫方可在不同位置移动，呼叫方用户请求与被叫方建立会话时，向SIP服务器发送请求查找找用户位置；
2. **用户有效性**：会话参与者可以有多名，在收到呼叫请求之后，可自愿选择是否参与会话；
3. **用户能力描述**：请求方在SDP中描述自身支持哪些媒体数据或会话参数；
4. **建立会话**：在用户发送会话请求时，必要的一些媒体参数已描述清楚后，被叫方用户接受请求，并根据请求信息中的SDP描述来配置后，需要发送“Ringing"振铃消息建立起会话；
5. **会话管理**：添加、修改媒体流参数或其他参数；保持会话等待、激活会话和终止现有会话。

### **SIP协议消息类型**

SIP协议的消息是基于文本的协议，标准的消息由开始行、消息头和消息体组成，分为请求消息和响应消息，区别在于消息的起始行是否包含状态信息。SIP请求消息定义了6种方法：

1. REGISTER注册：UAC启动时告诉注册服务器自身的位置
2. INVITE邀请：UA发起会话邀请
3. ACK: 被呼叫方在收到邀请后，回应200或180表示确认，ACK表示主叫方收到确认信息，会话建立成功
4. CANCEL:被呼叫方取消主叫方的会话邀请
5. BYE：会话参与者有一方想退出已建成功的会话，发起BYE消息
6. OPTIONS：查询代理服务器的能力和负载情况

响应消息有6种，包含临时应答、请求成功、位置异常通知、请求失败通知、服务器故障或全局错误

SIP协议支持消息扩展，消息类型、消息头和消息体都可以扩充，如以下几种情况

- 将电话信令集成到sip消息中，以解决公共交换电话网和sip网络和互联
- 解决跨网域的防火墙和NAT通信问题
- refer事件扩展，支持呼叫转移

### **SIP的主要组件：**

- **终端用户设备UA**：用于创建和管理 SIP协议会话的设备
- **位置服务器：**记录并管理UA的位置信息
- **注册服务器** ：为了判定用户的处于何地，UA终端注册到一个注册服务器，注册服务器会分配特定地址给到终端UA ，该地址是有生命周期的，UA需要定时刷新注册状态以保活。
- **代理服务器**：接受UA 的会话请求并查询注册服务器，获取收件方 UA 的地址信息。然后，它将会话邀请信息直接转发给收件方 UA（同一域中）或代理服务器（UA 位于另一域中）。根据对请求的处理方式分为有状态代理服务器和无状态代理服务器，无状态代理服务器仅对消息进行简单传递，有状态代理服务器会保留接收的请求和应答的相关信息，用于处理与该消息有关的后续消息。通过创建事务状态机对消息的全生命周期进行管理，每条都是会给事务状态机处理，直到事务完成。
- **重定向服务器**：接收请求后查询请求消息者的位置信息，然后在应答消息中创建位置列表请返回该信息，允许 SIP代理服务器将 SIP协议会话邀请信息定向到外部域。

### SIP会话过程

SIP协议同HTTP协议相似并采用了一些相同的设计原则如客户端UAC/服务端UAS模型，主动发起请求端为UAC端，接受请求端为UAS端，一个请求的完整称为一个事务，以一个或者多个响应结束，中间可以支持多个临时响应，在这个过程中UAC和UAS的角色是可以互换的，如一次呼叫事务中A发起呼叫，A是UAC，另一端是UAS；呼叫结束时谁先挂机谁是UAC，另一端是UAS。

SIP协议以INVITE请求，以SIP URL的方法表示发起，以BYE请求来结束。根据不同的网络结构分为三种方式：两个UA之间直接进行呼叫、通过代理服务器呼叫和通过重定向的方式呼叫。SIP一般需要使用以下协议完整一次完成的通信：

- SDP：会话名称、会话时间、 哪个IP端口、会话参与者所支持的多媒体种类、传输协议类型、编解码算法等信息
- RTP： 实时传输协议
- RTCP: 实时传输控制协议,和 RTP一起工作的控制协议
- TLS：提供安全性和完整性保障

## 2.什么是Pjsip？

PJSIP是一个用C语言编写的免费开源多媒体通信库， 实施基于标准的协议，如 SIP、SDP、RTP、STUN、TURN 和 ICE。 它将信令协议 （SIP） 与丰富的多媒体框架和 NAT 遍历相结合 功能转换为可移植的高级 API，适用于几乎任何类型的 系统范围从台式机、嵌入式系统到移动手机。

PJSIP既紧凑又功能丰富。它支持音频、视频、状态和即时 消息传递，并具有广泛的文档。PJSIP非常便携。在移动设备上， 它抽象了与系统相关的功能，并且在许多情况下能够利用本机 设备的多媒体功能。

#### [概述 — PJSIP Project 2.14-dev 文档](https://docs.pjsip.org/en/latest/overview/intro.html)

## 3.什么是Freeswitch？

FreeSWITCH是一个开源的电话软交换平台，主要开发语言是C，以[MPL1.1](http://www.opensource.org/licenses/mozilla1.1.php)发布。

它有很强的可伸缩性 ── 从最简单的软电话到商业级的软交换平台几乎无所不能。它支持SIP、H323、WebRTC等通信协议。另外，它还支持很多高级的SIP特性，如Presence、BLF、SLA以及TCP、TLS和sRTP 等。它可以作为SBC使用和多协议网关使用，也可以作为B2BUA连接其它的VoIP系统，如OpenSIPS、Kamailio、Asterisk等。

FreeSWITCH支持各种带宽的语音编解码，支持 8K、16K、32K及48KHz的高清通话，并可以在桥接不同频率的语音时自动进行转换。它可以运行在32位及64位的Windows、macOS、Linux以及Solaris等平台上，伸缩性极强，不管是一个信用卡大小的Raspberry Pi，还是数十、数百核的大型Linux服务器，都能运行FreeSWITCH。此外，它还可以运行于Docker容器及K8S云原生环境中。

## 如果无法注册，请使用wireshark抓包工具选择SIP消息排错或检查5060端口号是否被占用。
