# 七牛直播demo项目iOS版本（此版本只支持真机）


##打包APP下载

[点击下载](https://dn-devtools.qbox.me/QNPilePlayDemo-v1.1.1.html)

iOS点击下载之后，还无法直接运行，需要信任我们299账户的企业证书方可使用
操作如下：
点击设置—>通用—>设备管理
进入管理，显示Shanghai Qiniu Information Technologies Co.Ltd 的企业级应用证书，点击信任，即可使用下载app

##官方直播文档

[点击下载](http://devtools.qiniu.com/pili-guide-v1.pdf)

##项目简介

本项目模拟一个UGC场景的iOS
直播APP，项目的服务端在[qiniu-live-server](https://github.com/qiniudemo/qiniu-live-server)。

该项目是基于七牛直播技术，及七牛iOS端推流SDK开发的，七牛iOS端SDK地址为[PLCameraStreamingKit](https://github.com/pili-engineering/PLCameraStreamingKit)。

##七牛直播技术

七牛直播技术是七牛基于云存储推出的直播技术，该直播技术的特点在于，在客户端推流的过程中，流切片在分发给客户之前会自动在存储中保存一份，这样就方便后面的回看需求。

七牛提供了直播推流的客户端SDK，涵盖iOS，Android平台，支持RTMP协议推流，RTMP或HLS协议的直播观看。另外客户端推流的SDK还可以自动根据网络情况选择合适的推流参数，适应不同
网络环境下的直播需求。

##直播APP技术架构

直播APP涉及到如下几个方面的角色：

1. 直播业务服务器，该服务器主要是验证客户端的权限并在权限合法之后授权客户端推流参数，客户端使用推流参数进行推流。
2. 七牛直播系统，该系统主要根据直播业务服务器的请求来创建直播流，获取直播流信息，提取直播流回看地址等信息。
3. 推流客户端，推流客户端主要工作是从直播业务服务器获取直播推流参数，然后将录制的视频流推送到七牛直播系统。

**直播业务服务器**一般由客户自行开发，用来和七牛直播系统进行交互，七牛提供服务端的SDK，客户可以很方便地使用适合自己的编程语言的SDK开发包来开发服务端API。

**推流客户端**一般由客户自行开发，用来和直播业务服务器交互，将视频流推送到七牛直播系统或者从直播业务服务器获取观看地址，然后从七牛直播系统根据地址获取视频内容。

##直播APP业务流程

1. 直播APP登录帐号，该账号的合法性和其相关的业务逻辑由直播业务服务器提供和验证。
2. 直播APP从直播业务服务器获取推流的参数信息，准备使用集成在APP中的七牛推流SDK来将视频流推送到七牛直播系统
3. 直播APP从直播业务服务器获取推流参数后，在开始推流时，发送开始信号给业务服务器，业务服务器记录下该直播过程的起始时间，并生成唯一性id给客户端
4. 直播APP开始进行推流，推送的直播流数据将通过SDK直接发送到七牛直播系统，推流协议为RTMP。
5. 其他的直播APP客户可以从直播业务服务器获取当前直播的RTMP或者HLS的地址进行观看，RTMP的实时性要优于HLS，另外七牛提供的[直播播放器](https://github.com/pili-engineering/PLPlayerKit)支持RTMP协议。
6. 直播APP结束推流，同时发送停止推流信号给直播业务服务器，业务服务器记录下该直播过程的结束时间，可选性地让客户命名直播过程，方便未来回放。
7. 直播APP本身也可以获取已直播完成的视频播放地址进行回看。
