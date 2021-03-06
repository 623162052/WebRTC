# EasyRTC

一个出色的开源WebRTC套件。

这个套件基于HTML5和JavaScript实现，包含了EasyRTC服务器的安装和客户端API，运行于Node环境，支持在线多人视频，语音，文本信息发送等功能。

Editor: PengBaowen (November 2017 )

文章在线阅读：https://github.com/bovinphang/WebRTC/tree/master/demo/EasyRTC


## <a name='toc'>目录</a>


1. [什么是EasyRTC OpenSource](#What-Is-EasyRTC-OpenSource)
2. [特征](#Features)
3. [EasyRTC Open Source应用架构图](#Application-Architecture-Diagram)
4. [安装](#install)
5. [有用的术语](#Useful-Terminology)
6. [你的第一个应用程序](#Your-First-App)
7. [服务器部署 - “让我的客户端跑起来”](#Hosting-on-a-server)




## <a name='What-Is-EasyRTC-OpenSource'>1. 什么是EasyRTC OpenSource</a>

EasyRTC OpenSource是：

- 用JavaScript编写的浏览器客户端库。 该客户端处理信令，并在很大程度上将应用程序与WebRTC API中正在进行的更改隔离。
- 基于Node.js的信令服务器。Node.js可以运行在单核心Raspberry Pi（第一版）的云服务器上。

总之，这两个组件可以让你用几行简单的代码编写一个简单的视频会议应用程序或文件共享应用程序等等。

## <a name='Features'>2. 特征</a>

EasyRTC消除了开始使用WebRTC的痛苦，其有以下很酷的特征：

1. 跨浏览器支持。
2. 源代码执行。
3. 轻松安装服务器。EasyRTC服务器基于Node.js编写，它可以让你的应用程序运行于自己的机器或云服务器上，在Linux、Windows、或Mac系统上安装只需几分钟。使用EasyRTC的JavaScript客户端SDK可以让你快速的构建和部署WebRTC应用。
4. 使用WebSocket服务。
5. 服务器和客户端的单一语言（JavaScript）。EasyRTC 代码示例有助于缩短学习曲线，并让你快速掌握WebRTC的应用。
6. 免费开放源码。EasyRTC在BSD 2开源许可协议下运作，这意味着它的源代码是完全免费开放的，不用支付任何费用或其他隐藏费用。

## <a name='Application-Architecture-Diagram'>3. EasyRTC Open Source应用架构图</a>

![应用架构图](./images/Architecture-Diagram.png)


## <a name='install'>4. 安装</a>

安装一个EasyRTC服务器到你的工作站中（无论是台式机或笔记本电脑）是非常快速和简便的。在同一工作站中运行EasyRTC客户端也无需使用繁琐的SSL证书。这对于测试你的客户端应用程序和其他设备一起运行情况将是非常有用的；而运行在Chrome浏览器中的 WebRTC客户端是不能访问摄像头或麦克风的，除非它们是架构在SSL服务器或在同一台机器上（这是浏览器的安全约束）。

#### 安装EasyRTC

此处以在CentOS 7系统的安装过程为例。

1. 安装最新版本的[Node.js](https://nodejs.org/)(若已安装，略过此步骤)：

   ```shell
   [root@localhost src]# wget https://nodejs.org/dist/v8.7.0/node-v8.7.0.tar.gz
   [root@localhost src]# tar -zxvf node-v8.7.0.tar.gz
   [root@localhost src]# cd node-v8.7.0
   [root@localhost node-v8.7.0]# ./configure
   [root@localhost node-v8.7.0]# make
   [root@localhost node-v8.7.0]# make install
   [root@localhost node-v8.7.0]# ln -fs /usr/local/bin/node /sbin/node #让所有用户都可用node
   [root@localhost node-v8.7.0]# node -v #测试node是否安装成功
   ```

2. 安装[Git](https://git-scm.com/downloads)客户端(若已安装，略过此步骤)：

   ```shell
   [root@localhost src]# yum install git
   [root@localhost src]# git --version  #测试git是否安装成功
   ```

   或通过编译安装：

   ```shell
   [root@localhost src]# wget https://git-core.googlecode.com/files/git-1.9.0.tar.gz
   [root@localhost src]# tar -zxvf git-1.9.0.tar.gz
   [root@localhost src]# cd git-1.9.0
   [root@localhost git-1.9.0]# ./configure --prefix=/usr/local/git
   [root@localhost git-1.9.0]# make
   [root@localhost git-1.9.0]# make install
   [root@localhost git-1.9.0]# git --version  #测试git是否安装成功
   ```

   添加git到环境变量：

   ```shell
   [root@localhost src]# vi /etc/profile
   ```

   将其打开把：

   ```shell
   export PATH=$PATH:/usr/local/git/bin:/usr/local/git/libexec/git-core
   ```

   这句放在“export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE INPUTRC”的上一行。
   （因为bin目录只有4个命令，其它的几十个命令在libexec/git-core目录下，所以，在PATH搜索路径下，也要加上才能找到）
   立即生效环境配置，不需要重启，用下命令：

   ```shell
   [root@localhost src]# . /etc/profile
   ```

3. 从GitHub库获取 EasyRTC源码并安装项目依赖：

```shell
[root@localhost src]# git clone git@github.com:priologic/easyrtc.git
[root@localhost src]# cd easyrtc
[root@localhost easyrtc]# cnpm install
```
4. 安装server_example的项目依赖：

```shell
[root@localhost easyrtc]# cd server_example
[root@localhost server_example]# cnpm install
```

5. 如果你没有看到任何令人讨厌的错误，那么你应该能够通过下面的命令来启动EasyRTC服务器（在server_example目录中）：

```shell
[root@localhost server_example]# node server.js
```

​	默认情况下，服务器提供演示应用程序的端口为8080，所以如果你在同一台机器上打开一个兼容WebRTC的浏览器，并打开http://localhost:8080，它将把带到一个demo汇总页面。选择一个demo（简单的音频视频demo是一个不错的选择）。打开一个新的页签并打开同一个URL以获得demo的第二个实例，您就可以尝试双向通信了。

​	警告：如果你在同一个没有耳麦的实体房间运行相同的两个WebRTC应用实例，它们会向彼此输出音频，导致产生一个尖锐的回音。如果有人在这房间里头，建议先将扬声器静音。 

​	注意：Simple Video+Audio Demo显示本地摄像机的输出（这是一个很好的提醒，所以你不会在会议中做任何粗鲁的事情）和潜在的远程摄像机的输出。 当你在同一台机器上运行所有的东西时，你会注意到一个显示是相对于另一个显示的(即镜像显示的)。 如果您想知道为什么这样做，请尝试以下操作：

- 在摄像头前面抬起左手，并考虑哪个视频窗格看起来是正确的。
- 在摄像头前面保留一些打印的文本，并考虑哪个视频窗格看起来是正确的。


#### 使用pm2守护进程来开机自启动server.js

此处以在CentOS 7系统的安装过程为例。

1. 安装PM2：
```shell
[root@os11728 src]# npm install -g pm2
[root@os11728 src]# pm2 -V  #测试是否安装成功
2.7.2
```

2. 确认node的位置：
```shell
[root@localhost server_example]# which node
/usr/local/bin/node
或：
[root@localhost server_example]# whereis node
node: /usr/sbin/node /usr/local/bin/node
```

3. 启动node.js应用脚本程序：
```shell
[root@localhost server_example]# NODE_ENV=production pm2 start /var/www/root/easyrtc/server_example/server.js  --name 'EasyRtc server' -f -i 0
```

4. 保存脚本：
```shell
[root@localhost server_example]# pm2 save
```

5. 创建开机启动脚本：
```shell
[root@localhost server_example]# pm2 startup systemd
```

6. 重启服务器：
```shell
[root@localhost server_example]# reboot
```

7. 列出由pm2管理的所有进程信息（还会显示一个进程会被启动多少次，因为没处理的异常）：

```shell
[root@localhost ~]# pm2 ls #或：pm2 list
```
![pm2 list](./images/pm2-list.png)

8. 监视每个node进程的CPU和内存的使用情况：

```shell
[root@localhost ~]#pm2 monit
```
![pm2 monit](./images/pm2-monit.png)
9. pm2其它常用命令：

```shell
[root@localhost src]# pm2 logs  #显示所有进程日志
[root@localhost src]# pm2 stop all  #停止所有进程
[root@localhost src]# pm2 restart all  #重启所有进程
[root@localhost src]# pm2 reload all 0  #秒停机重载进程 (用于 NETWORKED 进程)
[root@localhost src]# pm2 stop 0  #停止指定的进程
[root@localhost src]# pm2 restart 0  #重启指定的进程
[root@localhost src]# pm2 startup  #产生 init 脚本 保持进程活着
[root@localhost src]# pm2 web  #运行健壮的 computer API endpoint (http://localhost:9615)
[root@localhost src]# pm2 delete 0  #杀死指定的进程
[root@localhost src]# pm2 delete all  #杀死全部进程

```
10. 运行进程的不同方式：

```shell
[root@localhost src]# pm2 start app.js -i max  #根据有效CPU数目启动最大进程数目
[root@localhost src]# pm2 start app.js -i 3  #启动3个进程
[root@localhost src]# pm2 start app.js -x  #用fork模式启动 app.js 而不是使用 cluster
[root@localhost src]# pm2 start app.js -x -- -a 23  #用fork模式启动 app.js 并且传递参数 (-a 23)
[root@localhost src]# pm2 start app.js --name serverone  #启动一个进程并把它命名为 serverone
[root@localhost src]# pm2 stop serverone  #停止 serverone 进程
[root@localhost src]# pm2 start app.json  #启动进程, 在 app.json里设置选项
[root@localhost src]# pm2 start app.js -i max -- -a 23  #在--之后给 app.js 传递参数
[root@localhost src]# pm2 start app.js -i max -e err.log -o out.log  #启动 并 生成一个配置文件

```

## <a name='Useful-Terminology'>5. 有用的术语</a>

- Video track（视频轨道）：表示单个视频或等效设备输出的对象
- Audio track（音频轨道）：表示单个麦克风或同等设备输出的对象

- Media Stream（媒体流）：用作包含一个或多个视频轨道和/或一个或多个音频轨道的对象。

- Data channel（数据通道）：用于将字符串或二进制数据从一个点发送到另一个点的对象。即在两者之间建立了一个双向数据通道的连接，实现点对点通信。

- Peer connection（对等连接）：作为两个点之间连接的对象，允许在网络上共享媒体流和数据通道。

- Easyrtcid：EasyRTC服务器上某个peer的唯一ID。

- Call（呼叫）：作为动词，表示与另一个点建立连接的行为，以便媒体流或数据可以被发送。 作为名词，表示有一个点对点连接的状态。

## <a name='Your-First-App'>6. 你的第一个应用程序</a>

我们简单的浏览器应用程序将遵循简单音视频演示的模型，其中包括两个部分：一个HTML文件，其定义一些用于显示媒体流的HTMLVideoObjects，以及一些用于启动呼叫的按钮。还有一个关于程序逻辑的JavaScript文件。

**HTML代码如下所示：**

```html
<!DOCTYPE html>
  <html>
      <head>
          <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
          <title>MyFirst app</title>
          <link rel="stylesheet" type="text/css" href="/easyrtc/easyrtc.css" />
          <script src="/socket.io/socket.io.js"></script>
          <script type="text/javascript" src="/easyrtc/easyrtc.js"></script>
          <script type="text/javascript" src="MyFirstApp.js"></script>
          <style>
              video {
                       width:320px;
                       height:240px;
                    }
              .divHolder {
                    position:relative;
                    float:left; 
                    background-color: blue;
                    margin: 1px;
                    }
          </style>
      </head>
      <body>
          <button onclick = "connect();"> Connect to server</button>
          <div id="otherClients">Peers:</div>
          <div>
              <div class="divHolder">
                <video autoplay="autoplay" class="easyrtcMirror" id="selfVideo" muted="muted" volume="0" ></video>
              </div>

              <div class="divHolder">
                <video autoplay="autoplay" id="callerVideo1"></video>
              </div>

              <div class="divHolder">
                <video autoplay="autoplay" id="callerVideo2"></video>
              </div>

              <div class="divHolder">
                <video autoplay="autoplay" id="callerVideo3"></video>
              </div>
          </div>
      </body>
  </html>
```

首先您会注意到对/easyrtc/easyrtc.css和/easyrtc/easyrtc.js的引用。 easyrtc.css和easyrtc.js这两文件实际上是位于api文件夹中的，但服务器已设置将/ easyrtc映射到api文件夹中。

easyrtc.js文件包含基本的Easyrtc方法和Easyrtc_App方法。 easyrtc.css文件包含制作视频镜像及定位视频上方的“close call”图标的css定义。 socket.io.js文件提供了与服务器的websocket通信。 MyFirstAp.js文件是我们的应用程序代码。

“connect()”方法将用于启动到服务器的连接。

id为“otherClients”的div将作为呼叫其它peer的按钮的容器。

包含每个视频对象的div是在其所包含的视频对象上定位“关闭呼叫”图标的结构的一部分。

**我们需要编写的JavaScript如下所示：**

```javascript
  function connect() {
      easyrtc.setRoomOccupantListener(convertListToButtons);
      easyrtc.easyApp("easyrtc.audioVideoSimple", "selfVideo", 
          ["callerVideo1", "callerVideo2", "callerVideo3"],
                      loginSuccess, loginFailure);
  }

  function convertListToButtons (roomName, data, isPrimary) {
      var otherClientDiv = document.getElementById('otherClients');
      otherClientDiv.innerHTML = "";
      for(var easyrtcid in data) {
          var button = document.createElement('button');

          button.onclick = function(easyrtcid) {
              return function() {
                  performCall(easyrtcid);
              };
          }(easyrtcid);

          var label = document.createTextNode(easyrtcid);
          button.appendChild(label);
          otherClientDiv.appendChild(button);
      }
  }

  function performCall(otherEasyrtcid) {
      var successCB = function() {};
      var failureCB = function() {};
      easyrtc.call(otherEasyrtcid, successCB, failureCB);
  }

  function loginSuccess(easyrtcid) {
      easyrtc.showError("none", "Successfully connected");
  }

  function loginFailure(errorCode, message) {
      easyrtc.showError(errorCode, message);
  }
```

这个connect方法做了两件事情：

- 设置了一个回调函数，只要连接到服务器的peer列表发生更改时，就会调用该回调函数。
- 发起一个到easyrtc服务器的连接，同时创建播放流媒体视频对象的列表。

convertListToButtons函数为服务器已知的每个peer生成一个按钮。每个按钮都有一个回调函数，用于生成对特定peer的调用。

performCall方法需要传递一个easyrtcid参数并请求对它的呼叫。当呼叫建立并且从当前peer接收到远程流媒体时，回调（由easyApp的方法隐式设置）将把该流媒体附加到第一个可用视频对象中。我们通过初始化连接的easyApp方法来设置回调；其中一个回调会将从peer接收到的流媒体附加到空闲的视频对象中，而另一个回调则在关联的流媒体结束时清除视频对象。

这个应用程序只涉及到EasyRTC API的皮毛，更多使用方法请参阅EasyRTC API手册。

## <a name='Hosting-on-a-server'>7. 服务器部署 - “让我的客户端跑起来”</a>

在服务器上部署您的应用程序需要做以下三个步骤：

- 更改服务器的端口，因为你可能不想使用8080端口。
- 使用SSL。
- 提供STUN和TURN服务器。

### 7.1 更改端口

要更改应用程序的服务端口，请用编辑器中打开server_example / server.js文件。 找到如下一行内容：

```javascript
var webServer = http.createServer（app）.listen（8080）;
```

这一行定义了你使用的服务器端口。 将8080更改为其他尚未使用的端口号。 请注意，在Linux系统上，对于非root权限用户是不能使用1024以下的端口的，因此强烈建议你使用大于1024的端口号。

### 7.2 使用SSL

​	如前所述，除非您的应用程序部署在本地主机或SSL服务器（https）上，否则在Chrome浏览器上运行的应用程序将无法访问本地摄像头和麦克风。 当您在进行开发时，通过node.js来处理SSL是非常简单的。 您可以在 [EasyRTC: Using SSL](https://www.easyrtc.com/docs/easyrtc_server_ssl.php)页面上找到详细的使用说明。

​	但是对于生产环境，您应该使用NGINX，HAProxy或类似的工具。 这些工具可以在处理SSL加密时将低编号的端口（如443，默认的https端口）映射到高编号的端口，这样您服务器上的代码就会看到像来自高编号端口的常规非SSL流量访问。 我们的经验是，与在easyrtc服务器内部处理SSL相比，它们提供更高的性能和更低的延迟。

### 7.3 提供STUN和TURN服务器

**为什么？是什么？**

WebRTC被称为点对点（P2P），所以这个技术对新手来说所面临的第一个问题是，“如果它是点对点的，为什么我还需要服务器？

在一个理想的世界里（从实现点对点应用的角度来看），我们所有的设备都有唯一的可以从互联网上的任何地方到达的地址。当然，由于存在NAT和对称结构NAT行为的路由器和防火墙，真实世界并不理想。