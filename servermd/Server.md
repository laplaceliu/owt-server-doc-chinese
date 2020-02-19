Open WebRTC Toolkit(OWT) 服务端用户指南
----------------------

# 1 概览 {#Conferencesection1}
## 1.1 介绍 {#Conferencesection1_1}
欢迎来到Open WebRTC Toolkit服务端用户指南。这份指南描述了如何安装、配置OWT服务器，使其能用于多方会议等目的。这份指南同时也解释了如何安装和运行Peer服务器来进行点对点通信。

Open WebRTC Toolkit服务器提供了一个高效的基于WebRTC的视频会议服务，这个服务可以把一个单一的WebRTC流扩展到多个端点。下面的列表简要地说明了这份文档中每个小节的目标：

 - 小节1. 这份指南中用到的介绍性文字和约定
 - 小节2. 安装和配置OWT服务器
 - 小节3. OWT服务器管理控制台的简明指南
 - 小节4. 安装OWT示例应用服务器
 - 小节5. 安装和运行Peer服务器

OWT服务器、示例应用服务器、以及peer服务器在安装过程中的必要条件和依赖项，会在相应的小节进行描述。

##1.2 术语 {#Conferencesection1_2}
本手册用到了以下的缩略词和术语：

  缩写       |  完整名称
-------------|--------------
ADT|Android Developer Toolkit
API|Application Programming Interface
GPU|Graphics Processing Unit
IDE|Integrated Development Environment
JS|JavaScript programming language
MCU|Multipoint Control Unit
MSML|Media Server Markup Language
P2P|Peer-to-Peer
QoS|Quality of Service
ReST|Representational State Transfer
RTC|Real-Time Communication
RTCP|RTP Control Protocol
RTSP|Real Time Streaming Protocol
RTMP|Real Time Messaging Protocol
RTP|Real Time Transport Protocol
SDK|Software Development Kit
SDP|Session Description Protocol
SIP|Session Initiation Protocol
XMPP|Extensible Messaging and Presence Protocol
WebRTC|Web Real-Time Communication

##1.3 更多信息 {#Conferencesection1_3}
如果想获取更多信息，可以访问以下网页：

 - Intel HTML Developer Zone: https://software.intel.com/en-us/html5/tools
 - Intel<sup>®</sup> Collaboration Suite for WebRTC
    1. http://webrtc.intel.com
    2. https://software.intel.com/en-us/forums/webrtc
    3. https://software.intel.com/zh-cn/forums/webrtc
 - Intel<sup>®</sup> Visual Compute Accelerator
    1. https://www.intel.com/content/www/us/en/servers/media-and-graphics/visual-compute-accelerator.html
    2. https://www-ssl.intel.com/content/www/us/en/cloud-computing/visual-cloud.html
 - The Internet Engineering Task Force (IETF<sup>®</sup>) Working Group
    1. https://tools.ietf.org/wg/rtcweb/
 - W3C WebRTC Working Group: http://www.w3.org/2011/04/webrtc/
 - WebRTC Open Project: http://www.webrtc.org

# 2 OWT服务器的安装 {#Conferencesection2}

## 2.1 介绍 {#Conferencesection2_1}

本小节描述了安装OWT服务器时系统的必要项，和对它的客户端的兼容性。

> **注意**:    安装Peer服务器的必要条件在这份指南的<a href="#Conferencesection5">第5小节</a>。

## 2.2 必要条件和兼容性 {#Conferencesection2_2}

表2-1描述了安装OWT服务器时系统的必要项。 describes the system requirements for installing the OWT server. 表2-2 gives an overview of OWT server compatibility with the client.

**Table 2-1. Server requirements**
Application name|OS version
-------------|--------------
OWT服务器|CentOS* 7.6, Ubuntu 18.04 LTS

GPU加速只能在内核4.14或更高版本下启用（推荐4.19或更高版本）。

如果你想设置视频会议服务
If you want to set up video conference service with H.264 codec support powered by non GPU-accelerated OWT server, OpenH264 library is required. See [Deploy Cisco OpenH264* Library](#Conferencesection2_3_4) section for more details.

If you want to set up video conference service with SVT-HEVC Encoder on Ubuntu 18.04 LTS. See [Deploy SVT-HEVC Encoder Library](#Conferencesection2_3_6) section for more details.

If you want to set up video conference service powered by GPU-accelerated OWT server through Intel® Media SDK, please follow the below instructions to install server side SDK where the video-agents run.

如果你正工作在以下平台，并且使用的是集成显卡，请先安装Intel® Media SDK。目前最新的发布版在MediaSDK 2018 Q4上进行了全面的测试(https://github.com/Intel-Media-SDK/MediaSDK/releases/tag/intel-mediasdk-18.4.0)。

 - Intel® Xeon® E3-1200 v4 Family with C226 chipset
 - Intel® Xeon® E3-1200 and E3-1500 v5 Family with C236 chipset
 - 5th Generation Intel® CoreTM
 - 6th Generation Intel® CoreTM
 - 7th Generation Intel® CoreTM

访问https://github.com/Intel-Media-SDK/MediaSDK查看下载和安装的指令。

外部流输出、mp4格式的录像依赖于AAC编码器libfdk_aac（这个编码器在ffmpeg库中），具体请见：[编译和部署带libfdk_aac编码器的ffmpeg库](#Conferencesection2_3_5) section for detailed instructions.

 **Table 2-2. Client compatibility**
Application Name|Google Chrome\* 78|Mozilla Firefox\* 70|Microsoft Edge\* 44.18362.387.0|Safari\* 13.0|Open WebRTC Toolkit Client SDK for Android | Open WebRTC Toolkit Client SDK for iOS | Open WebRTC Toolkit Client SDK for Windows
--------|--------|--------|--------|--------|--------|--------|--------
OWT Client|YES|YES|YES|YES|YES|YES|YES
Management Console|YES|YES|YES|YES|N/A|N/A|N/A

## 2.3 安装OWT服务器 {#Conferencesection2_3}
本小节描述了安装OWT服务器的依赖项和步骤。
### 2.3.1 依赖项 {#Conferencesection2_3_1}
**Table 2-3. OWT服务器的依赖项**
名称|版本|附注
--------|--------|--------
Node.js |8.15.0|Website: http://nodejs.org/
Node modules|指定的版本|N/A
MongoDB| 2.6.10 |Website: http://mongodb.org
System libraries|最新的|N/A

除了系统库，所有的依赖项，要么包含在发布版的包中，要么可以通过发布版的包进行自动安装。

当你使用Ubuntu或CentOS的包管理器安装OWT服务器包时，所有必要的系统库都会被安装上。

要保证安装OWT服务器之前已经安装了Node.js。我们推荐的版本是8.15.0。参阅http://nodejs.org/获取更多细节以及如何安装。

在安装OWT服务器之前，确保你的登录用户有系统权限，换句话说就是能执行`sudo`。

### 2.3.2 配置OWT服务器的机器 {#Conferencesection2_3_2}

如果你的服务器运行在CentOS上，需要配置系统防火墙来保证所有OWT服务器组件需要的端口是打开的。

### 2.3.3 安装OWT服务器的包 {#Conferencesection2_3_3}

在服务器的机器上，解压压缩包。

~~~~~~{.sh}
    tar xf CS_WebRTC_Conference_Server_MCU.v<Version>.CentOS.tgz
~~~~~~

对于Ubuntu版的OWT服务器，则这样做：

~~~~~~{.sh}
    tar xf CS_WebRTC_Conference_Server_MCU.v<Version>.Ubuntu.tgz
~~~~~~

### 2.3.4 部署Cisco OpenH264*库 {#Conferencesection2_3_4}
默认安装的H.264库是一个没有任何媒体处理逻辑的“伪库”。
 To enable H.264 support in non GPU-accelerated OWT server system, you must deploy the Cisco OpenH264 library. Choose yes to download and enable Cisco Open H264 library during video-agent dependency installation at Release-<Version>/[video_agent/analytics_agent]/install_deps.sh.

Or you can also use install_openh264.sh or uninstall_openh264.sh scripts under Release-<Version>/video_agent folder to enable or disable Cisco OpenH264 library later.

### 2.3.5 编译和部署带libfdk_aac编码器的ffmpeg库 {#Conferencesection2_3_5}

The default ffmpeg library used by OWT server has no libfdk_aac support. If you want to enable libfdk_aac for external stream output or mp4 format recording, please compile and deploy ffmpeg yourself with following steps:

   > **Note**: The libfdk_aac is designated as "non-free", please make sure you have got proper authority before using it.

1. Go to Release-<Version>/audio_agent folder, compile ffmpeg with libfdk_aac with below command:

        compile_ffmpeg_with_libfdkaac.sh
> **Note**: This compiling script will install all dependencies for ffmpeg with libfdk_aac. If those dependencies are not expected on deployment machines, please run the script on other proper machine.

2. Copy all output libraries under ffmpeg_libfdkaac_lib folder to Release-<Version>/audio_agent/lib to replace the existing ones.

### 2.3.6 Deploy SVT-HEVC Encoder Library {#Conferencesection2_3_6}
The default SVT-HEVC Encoder library installed is a pseudo one without any media logic. If you want to enable SVT-HEVC Encoder, please compile and deploy the library yourself with following steps:

1. Go to Release-<Version>/video_agent folder, compile SVT-HEVC Encoder library with below command:

        compile_svtHevcEncoder.sh

2. Copy the built library(libSvtHevcEnc.so.1) to Release-<Version>/video_agent/lib to replace the existing one.

### 2.3.7 Use your own certificate {#Conferencesection2_3_7}

The default certificate (certificate.pfx) for the OWT server is located in the Release-<Version>/<Component>/cert folder. When using HTTPS and/or secure socket.io connection, you should use your own certificate for each server. First, you should edit management_api/management_api.toml, webrtc_agent/agent.toml, portal/portal.toml, management_console/management_console.toml to provide the path of each certificate for each server, under the key keystorePath. See Table 2-4 for details.

We use PFX formatted certificates in OWT server. See https://nodejs.org/api/tls.html for how to generate a self-signed certificate by openssl utility. We recommend using 2048-bit private key for the certificates. But if you meet DTLS SSL connection error in webrtc-agent, please use 1024-bit instead of 2048-bit private key because of a known network MTU issue.

After editing the configuration file, you should run `./initcert.js` inside each component to input your passphrases for the certificates, which would then store them in an encrypted file. Be aware that you should have node binary in your shell's $PATH to run the JS script.

 **Table 2-4. OWT server certificates configuration**
|  |configuration file|
|--------|--------|
| management-api HTTPS | management_api/management_api.toml |
| portal secured Socket.io | portal/portal.toml |
| DTLS-SRTP | webrtc_agent/agent.toml |
| management-console HTTPS | management_console/management_console.toml |

For OWT sample application's certificate configuration, please follow the instruction file 'README.md' located at Release-<Version>/extras/basic_example/.

### 2.3.8 Launch the OWT server as single node {#Conferencesection2_3_8}
To launch the OWT server on one machine, follow steps below:

1. Initialize the OWT package for the first time execution.

    For general OWT Server installation, use following command:

        bin/init-all.sh [--deps]

    If you want to enable GPU-acceleration through Intel Media Server Studio, use following command:

        bin/init-all.sh [--deps] --hardware
   > **Note**: If you have already installed the required system libraries, then --deps is not required. If you have installed early version of OWT server, the stored data will not be compatible with 4.1 version. Pay attention to the warning and choose option to update your OWT data in mongodb.

2. Run the following commands to start the OWT server:

        cd Release-<Version>/
        bin/start-all.sh
   > **Note**: If you want to run start-all in a combined command like "ssh remote-host OWT-server-installed-path/bin/start-all", the environment $DISPLAY needs to be explicitly specified as "export DISPLAY=:0.0".

3. To verify whether the server has started successfully, launch your browser and connect to the OWT server at https://XXXXX:3004. Replace XXXXX with the IP address or machine name of your OWT server.

> **Note**: The procedures in this guide use the default room in the sample.

### 2.3.9 Stop the OWT server {#Conferencesection2_3_9}
Run the following commands to stop the OWT server:

    cd Release-<Version>/
    bin/stop-all.sh

### 2.3.10 Set up the OWT server cluster {#Conferencesection2_3_10}
 **Table 2-5. Distributed OWT server components**
Component Name|Deployment Number|Responsibility
--------|--------|--------
management-api|1 or many|The entrance of OWT service, keeping the configurations of all rooms, generating and verifying the tokens. Application can implement load balancing strategy across multiple management-api instances
cluster-manager|1 or many|The manager of all active workers in the cluster, checking their lives, scheduling workers with the specified purposes according to the configured policies. If one has been elected as master, it will provide service; others will be standby
portal|1 or many|The signaling server, handling service requests from Socket.IO clients
conference-agent|1 or many|This agent handles room controller logics
webrtc-agent|1 or many|This agent spawning webrtc accessing nodes which establish peer-connections with webrtc clients, receive media streams from and send media streams to webrtc clients
streaming-agent|0 or many|This agent spawning streaming accessing nodes which pull external streams from sources and push streams to rtmp/rtsp destinations
recording-agent|0 or many|This agent spawning recording nodes which record the specified audio/video streams to permanent storage facilities
audio-agent|1 or many|This agent spawning audio processing nodes which perform audio transcoding and mixing
video-agent|1 or many|This agent spawning video processing nodes which perform video transcoding and mixing
analytics-agent|0 or many|This agent spawning media analyzing nodes which perform media analytics
sip-agent|0 or many|This agent spawning sip processing nodes which handle sip connections
sip-portal|0 or 1|The portal for initializing rooms' sip settings and scheduling sip agents to serve for them
app|0 or 1|The sample web application for reference, users should use their own application server
management-console|0 or 1|The console for conference management

Follow the steps below to set up a OWT server cluster:

1. Make sure you have installed the OWT server package on each machine before launching the cluster which has been described in section [Install the OWT server package](#Conferencesection2_3_3).

2. Choose machines to initialize MongoDB and RabbitMQ servers.

    For MongoDB, do as following:

        cd Release-<Version>/
        bin/init-mongodb.sh [--deps]

    For RabbitMQ, do as following:

        cd Release-<Version>/
        bin/init-rabbitmq.sh [--deps]
   > **Note**: You can change the shell scripts to initialize them according to your own requirement. Or choose any other existing MongoDB or RabbitMQ service, like those with cluster support. Make sure MongoDB and RabbitMQ services are started prior to all OWT server cluster nodes.

    If non-default user of rabbitmq-server or mongodb-server is used, do as following in each module:

        initauth.js

3. Choose machines to run management-api instances. These machines must be visible to clients(such as browsers and mobile apps).

4. Edit the configuration items of management-api in Release-<Version>/management_api/management_api.toml.

    - Make sure the [mongo.dataBaseURL] points to the MongoDB instance.
    - Make sure the [rabbit.port] and [rabbit.host] point to the RabbitMQ server.

5. Initialize and run management-api instance on each machine with following steps.

    1) Initialize OWT server manager management-api for the first time execution:

        cd Release-<Version>/
        management_api/init.sh

    **Note**: If you have installed early version of OWT server, the stored data will not be compatible with 4.0 version. Pay attention to the warning and choose option to update your OWT server data in mongodb.

    2) Run OWT server manager management-api and the sample application with following commands:

        cd Release-<Version>/
        bin/daemon.sh start management-api

6. Choose any management-api instance machine to run the sample application server for quick OWT service validation.

        cd Release-<Version>/
        bin/daemon.sh start app
   > **Note**: You can also deploy the sample application server on separated machine, follow instructions at Release-<Version>/extras/basic_example/README.md

7. Choose machines to run cluster-managers. These machines do not need to be visible to clients, but should be visible to management-api and all workers.
8. Edit the configurations of cluster-manager in Release-<Version>/cluster_manager/cluster_manager.toml.

    - Make sure the [rabbit.port] and [rabbit.host] point to the RabbitMQ server.

9. Run the cluster-manager on each machine with following commands:

        cd Release-<Version>/
        bin/daemon.sh start cluster-manager

10. Choose worker machines to run portals. These machines must be visible to clients.
11. Edit the configuration items of portal in Release-<Version>/portal/portal.toml.
    - Make sure the [mongo.dataBaseURL] points to the MongoDB instance.
    - Make sure the [rabbit.port] and [rabbit.host] point to the RabbitMQ server.
    - Make sure the [portal.ip_address] or [portal.networkInterface] points to the correct network interface which the clients’ signaling and control messages are expected to connect through.
    - Make sure the [portal.force_tls_v12] is true if you want to force TLS version not less than 1.2.

12. Run the portal on each machine with following commands:

        cd Release-<Version>/
        bin/daemon.sh start portal

13. Choose a worker machine to run conference-agent and/or webrtc-agent and/or streaming-agent and/or recording-agent and/or audio-agent and/or video-agent and/or sip-agent. This machine must be visible to other agent machines. If webrtc-agent or sip-agent is running on it, it must be visible to clients.

    - If you want to use Intel<sup>®</sup> Visual Compute Accelerator (VCA) to run video agents, please follow section [Configure VCA nodes](#Conferencesection2_3_11) to enable nodes of Intel VCA as a visible separated machine.

14. Edit the configuration items in Release-<Version>/{audio, video, conference, webrtc, streaming, recording, sip}_agent/agent.toml.
    - Make sure the [rabbit.port] and [rabbit.host] point to the RabbitMQ server.
    - Make sure the [cluster.ip_address] or [cluster.network_interface] points to the correct network interface through which the media streams will flow to other cluster nodes.

    Special for conference-agent, edit conference_agent/agent.toml
    - Make sure the [mongo.dataBaseURL] points to the MongoDB instance.


15. Initialize and run agent worker.

    1) Initialize agent workers for the first time execution if necessary

       For each agent, follow these steps:

            cd Release-<Version>/
            [video_agent/audio_agent/webrtc_agent/recording_agent/streaming_agent/sip_agent/analytics_agent]/install_deps.sh

       If you want to enable GPU-acceleration for video-agent through Media Server Studio, follow these steps:

            cd Release-<Version>/
            video_agent/install_deps.sh --hardware
            video_agent/init.sh --hardware

    2) Run the following commands to launch agent worker:

        cd Release-<Version>/
        bin/daemon.sh start [conference-agent/webrtc-agent/streaming-agent/audio-agent/video-agent/analytics-agent/recording-agent/sip-agent]

16. Repeat step 13 to 15 to launch as many OWT server agent worker machines as you need.

17. Choose one worker machine to run sip-portal if sip-agent workers are deployed:

        cd Release-<Version>/
        bin/daemon.sh start sip-portal

18. Choose one worker machine to run management-console if you need to create/update/delete services and rooms on web page.:

        cd Release-<Version>/
        bin/daemon.sh start management-console

### 2.3.11 Configure VCA nodes as seperated machines to run video-agent {#Conferencesection2_3_11}
To setup VCA nodes as separate machines, two approaches are provided. One is the network bridging provided by VCA software stack. The other is IP forwarding rules setting through iptables.

VCA built-in software stack provides network bridging support. Follow section 4.4.1 - Configuring nodes for bridged mode operation in VCA_SoftwareUserGuide.pdf. In this approach, all network traffic will go through one Ethernet interface.

If you want to map each VCA node to different Ethernet interface, IP forwarding can be one alternative to achieve this goal. Follow these steps:
1. Make sure one VCA card is correctly installed and VCA nodes successfully boot up.
2. Make sure the host machine has enough ethernet interface for VCA nodes. Eg: host IP is "10.239.44.100" and 3 ethernet interfaces for 3 nodes of 1 VCA card, and the IP of ethernet Interfaces are "10.239.44.1", "10.239.44.2", "10.239.44.3".
3. Make sure your ip routing tables(please get it with "route -n") on host machine are like below :

        Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
        0.0.0.0         10.239.44.241   0.0.0.0         UG    100    0        0 enp132s0f0
        10.239.27.228   10.239.44.241   255.255.255.255 UGH   100    0        0 enp132s0f0
        10.239.44.0     0.0.0.0         255.255.255.0   U     0      0        0 enp132s0f0
        172.31.1.0      0.0.0.0         255.255.255.0   U     0      0        0 eth0
        172.31.2.0      0.0.0.0         255.255.255.0   U     0      0        0 eth2
        172.31.3.0      0.0.0.0         255.255.255.0   U     0      0        0 eth1
   > **Note**: 10.239.27.228 is DNS server. enp132s0f0 is used for host machine and IP is "10.239.44.100".
4. Please use "ifconfig" on host machine of VCA card to list IP of VCA nodes, you will get the VCA node IPs like "172.31.1.254", "172.31.2.254", "172.31.3.254".
5. Configure IP soft routing policy to set VCA Node to seperated machine.
    + 5.1 Enable ip forward and clear iptables on host machine with the following commands:

            echo "1" > /proc/sys/net/ipv4/ip_forward
            iptables -t nat -P PREROUTING ACCEPT
            iptables -t nat -P POSTROUTING ACCEPT
            iptables -t nat -P OUTPUT ACCEPT

    + 5.2 Enable nat policy on host for 3 VCA nodes:

            iptables -t nat -A PREROUTING -d 10.239.44.1 -j DNAT --to-destination 172.31.1.1
            iptables -t nat -A POSTROUTING -s 172.31.1.1 -j SNAT --to-source 10.239.44.1
            iptables -A OUTPUT -d 10.239.44.1 -j DNAT --to-destination 172.31.1.1
            iptables -t nat -A PREROUTING -d 10.239.44.2 -j DNAT --to-destination 172.31.2.1
            iptables -t nat -A POSTROUTING -s 172.31.2.1 -j SNAT --to-source 10.239.44.2
            iptables -A OUTPUT -d 10.239.44.2 -j DNAT --to-destination 172.31.2.1
            iptables -t nat -A PREROUTING -d 10.239.44.3 -j DNAT --to-destination 172.31.3.1
            iptables -t nat -A POSTROUTING -s 172.31.3.1 -j SNAT --to-source 10.239.44.3
            iptables -A OUTPUT -d 10.239.44.3 -j DNAT --to-destination 172.31.3.1

    + 5.3 SSH to 3 seperated machines of VCA node:

            ssh root@10.239.44.1
            ssh root@10.239.44.2
            ssh root@10.239.44.3

    + 5.4 Disable firewall for VCA nodes after ssh login to it:

            systemctl stop firewalld
            systemctl disable firewalld

### 2.3.12 Stop the OWT server cluster {#Conferencesection2_3_12}

To stop the OWT server cluster, follow these steps:
1. Run the following commands on each management-api node to stop management-api instances:

        cd Release-<Version>/
        bin/daemon.sh stop management-api

    If sample application server also runs with specific management-api instance, run the following command to stop it.

        bin/daemon.sh stop app

2. Run the following commands on cluster manager machine to stop the cluster manager:

        cd Release-<Version>/
        bin/daemon.sh stop cluster-manager

3. Run the following commands on worker machines to stop cluster workers:

        cd Release-<Version>/
        bin/daemon.sh stop [portal/conference-agent/webrtc-agent/streaming-agent/audio-agent/video-agent/recording-agent/analytics-agent/sip-agent/sip-portal]

4. Run the following commands on management-console machine to stop the management-console:

        cd Release-<Version>/
        bin/daemon.sh stop management-console

### 2.4 OWT Server cluster’s fault tolerance / resilience {#Conferencesection2_4}

Open WebRTC Toolkit server provides built-in fault tolerance / resilience support for its key components, as Table 2-6 shows.

 **Table 2-6. OWT Server cluster components’ fault tolerance / resilience**
Component Name|Server Reaction|Client Awareness
--------|--------|--------
management-api|Multiple management-api instances provide stateless services at the same time. If application implements node failure detection and rescheduling strategy, when one node fails, other nodes should take over when the further requests are assigned to any of them.|Management-API RESTful request fail
cluster-manager|Auto elect another cluster-manager node as master and provide service.|Transparent
portal|All signaling connections on this portal will be disconnected, all actions through this portal will be dropped. Client needs to re-login the session.|server-disconnected event
conference-agent/node|All sessions impacted will be destroyed and all their participants will be forced disconnected and all actions will be dropped. Client needs to re-login the session.|server-disconnected event
webrtc-agent/node|All webrtc stream actions assign to this node will be dropped. Client needs to redo these actions.|stream-failed event
streaming-agent/node|All external stream actions assign to this node will be dropped. Client needs to redo these actions.|stream-failed event
recording-agent/node|All recording stream actions assign to this node will be dropped. Client needs to redo these actions.|stream-failed event
audio-agent/node|Auto schedule new audio-agent/node resource to recover the session context.|Transparent
video-agent/node|Auto schedule new video-agent/node resource to recover the session context.|Transparent
analytics-agent/node|All analytics stream actions assign to this node will be dropped. Client needs to redo these actions.|stream-failed event
sip-agent/node|All sip participants it carries should be dropped by session nodes.|SIP BYE signaling event

## 2.5 OWT Server configurations for public access {#Conferencesection2_5}

Open WebRTC Toolkit server provides the following settings in configuration files to configure the network interfaces for public access.

 **Table 2-7. Configuration Items for Public Access**
Configuration Item|Location|Usage
--------|--------|--------
webrtc.network_interfaces | webrtc_agent/agent.toml | The network interfaces of webrtc-agent that clients in public network can connect to
webrtc.minport | webrtc_agent/agent.toml | The webrtc port range lowerbound for clients to connect through UDP
webrtc.maxport | webrtc_agent/agent.toml | The webrtc port range upperbound for clients to connect through UDP
management-api.port | management_api/management_api.toml | The port of management-api should be accessible in public network through TCP
portal.hostname, portal.ip_address | portal/portal.toml | The hostname and IP address of portal for public access; hostname first if it is not empty.
portal.port | portal/portal.toml | The port of portal for public access through TCP

## 2.6 Security Recommendations {#Conferencesection2_6}
Intel Corporation does not host any conference cluster/service. Instead, the entire suite is provided so you can build your own video conference system and host your own server cluster.

Customers must be familiar with industry standards and best practices for deploying server clusters. Intel Corporation assumes no responsibility for loss caused from potential improper deployment and mismanagement.

The following instructions are provided only as recommendations regarding security best practices and by no means are they fully complete:

1. For the key pair access on OWT server, make sure only people with high enough privilege can have the clearance.
2. Regular system state audits or system change auto-detection. For example, OWT server system changes notification mechanism by third-party tool.
3. Establish policy of file based operation history for the tracking purpose.
4. Establish policy disallowing saving credentials for remote system access on OWT server.
5. Use a policy for account revocation when appropriate and regular password expire.
6. Use only per user account credentials, not account groups on OWT server.
7. Automated virus scans using approved software on OWT server.
8. Establish policy designed to ensure only allowed use of the server for external connections.
9. OWT server should install only approved software and required components, and verify default features status. For example, only enable what are needed.
10. Use a firewall and close any ports not specifically used on OWT server.
11. Run a vulnerability scan regularly for checking software updates against trusted databases, and monitor publically communicated findings and patch OWT system immediately.
12. Configure appropriate connection attempt timeouts and automated IP filtering, in order to do the rejection of unauthenticated requests.
13. If all possible, use or develop an automated health monitoring component of server service availability on OWT server.
14. Use a log analyzer solution to help detect attack attempts.
15. During server development, avoid processing of unexpected headers or query string variables, and only read those defined for the API and limit overall message size.
16. Avoid excessive error specificity and verbose details that reveal how the OWT server works.
17. During server and client development, suggest running some kind of security development lifecycle/process and conduct internal and external security testing and static code analysis with tools by trusted roles.
18. If all possible, backup all the log data to a dedicated log server with protection.
19. Deploy OWT cluster(Managers + Workers) inside Demilitarized Zone(DMZ) area, utilize external firewall A to protect them against possible attacks, e.g. DoS attack; Deploy Mongo DB and RabbitMQ behind DMZ, configure internal firewall B to make sure only cluster machines can connect to RabbitMQ server and access to MongoDB data resources.

**Figure 2-1. Security Recommendations Picture**
![Security Recommendations Picture](./pic/deploy.png)

## 2.7 FAQ {#Conferencesection2_7}
1. Sudden low volume when connecting Chrome on Windows to OWT server

    **Resolution**:

    Both the Chrome browser and Windows system itself can reduce the volume during a connection to the OWT server. To resolve this issue, disable the following Communications feature found in the Sound Settings using the Windows Control Panel.

    **Figure 2-2. Sound Settings**
    ![Sound Settings](./pic/soundsettings.png)
2. Failed to start OWT server, and receive the following error message: "child_process.js:948; throw errnoException(process._errno, 'spawn'); Error: spawn EMFILE"

    **Resolution**:

    Use the proper Node.js version as outlined in the [Dependencies](#Conferencesection2_3_1) section.

3. Failed to start OWT server, and receive the following error message: "Creating superservice in localhost/management-apidb SuperService ID: Sat Feb 7 19:10:32.456 TypeError: Cannot read property '_id' of null SuperService KEY: Sat Feb  7 19:10:32.479 TypeError: Cannot read property 'key' of null"

    **Resolution**:

    Use the proper MongoDB version as outlined in the [Dependencies](#Conferencesection2_3_1) section.
4. Run into network port conflicts on OWT server, and probably some error message like "net::ERR_CONNECTION_TIMED_OUT" or "ERROR: server connection failed: connection_error"

    **Resolution**:

    In the OWT server, the following default ports have been assigned for server usage: 5672 (configurable), 8080 (configurable), 3000 (configurable), 3300 (configurable). Make sure they are always available for OWT server. Also, in order to configure the two configurable ports above to a value smaller than the 1024 limitation, use the following command to enable it:

        sudo setcap cap_net_bind_service=+ep $(which node)

    If you are still not able to bypass the 1024 port limitation, remember to put the **OWT server library path into /etc/ld.so.conf.d**.
# 3 OWT服务器管理控制台的简明指南 {#Conferencesection3}
## 3.1 介绍 {#Conferencesection3_1}
The OWT Server Management Console is the frontend console to manage the OWT server. It is built with OWT's server-side APIs and it provides the management interface to OWT administrators.
## 3.2 访问 {#Conferencesection3_2}
OWT服务器运行起来之后，默认情况下你就可以在浏览器里通过https://XXXX:3300/console/访问控制台。为了访问这个服务，你会被问及service-id和service-key。

在弹出的对话框里填入service-id和service-key后，选择'remember me'然后点击'save changes'，这会保存你的会话。如果你想切换到其他服务，点击右上角的'profile'按钮，在弹出的对话框里填入新的service-id和service-key。如果你想要退出，则在弹出的对话框中点击红色的‘clear cookie'按钮。

如果还没有运行OWT服务器，那你在运行管理控制台之前，应该运行management-api服务器：

    cd Release-<Version>/
    bin/daemon.sh start management-api
    bin/daemon.sh start management-console

## 3.3 源码 {#Conferencesection3_3}
管理控制台的源码在Release-<Version>/management_console/public/目录。
## 3.4 服务管理 {#Conferencesection3_4}
只有super service user可以访问service management，在'overview'标签创建或删除服务。服务是一个实例，这种实例可以拥有房间，并且有能力管理房间。
> **注意**: 超级服务不能被删除，在management_api/management_api.toml可以对超级服务进行配置。

## 3.5 房间管理 {#Conferencesection3_5}
在一个服务中，任意的服务用户都可以管理房间，包括对房间的创建、删除、修改。

用户可以在自己的偏好设置中编辑房间的配置项来修改房间。下表列出的是房间的每一个配置项的细节：

 **表3-1. 房间的配置项**
条目|描述
--------|------------------
name | 房间的名字 (名字不同于ID，房间一旦创建，其ID不能被修改)
inputLimit | The input limitation for the room, means how many publication can a room allow
participantLimit | The max participant number of the room
roles | The role definition list for the room, for the description of list element see the role.*
role.role | The name for a certain role
role.*(operation).*(mediaType) | The capability to publish/subscribe audio/video stream for a certain role
views | The view list for the room, each view represents a combination of mix stream settings. To disable mixing, set this to empty list
view.label | The label for a certain view, two view labels in one room cannot be duplicated
view.audio.format | The default audio format of the view selected from 'mediaOut.audio' configuration
view.audio.vad | The 'activeInput' event will be emitted if this option is true, note that if a client publish more than one audio stream, this may not work well
view.video.format | The default video format of the view selected from 'mediaOut.format.video' configuration
view.video.parameters.resolution | The default video resolution of the view
view.video.parameters.framerate | The default video framerate of the view
view.video.parameters.bitrate | The default video bitrate of the view, if it's not specified, the mix engine will generate a default one, see Table 3-3 for more details
view.video.parameters.keyFrameInterval | The default video key frame interval of the view
view.video.maxInput | This indicates the maximum number of video inputs for the video layout definition, input that exceed the value will not be shown in mixed stream
view.video.motionFactor | The video motion factor is used to affact the default bitrate of the video stream
view.video.bgColor | The RGB representation for background color of the mixed video stream
view.keepActiveInputPrimary | The active input will always be shown in the 'primary' region when this option is set to true with 'vad' also enabled
view.layout | The layout of mixed video stream
view.layout.fitPolicy | The fit policy for input that does not perfectly match the width/height ratio
view.layout.templates | The layout template for the mixed video stream, a user can choose a base layout template and customize its own preferred ones, which would be combined as a whole for rendering mixed video
view.layout.templates.base | The template base for video layout
view.layout.templates.custom | The customized video layout uppon the base, see the [Section 3.5.1](#Conferencesection3_5_1)
mediaIn | The audio/video format that the room can accept, see the Table 3-2 for supported format
mediaOut | The audio/video format and parameters that the room can generate through media processing, see the Table 3-2 for supported formats
transcoding | The transcoding switch on audio, video format and parameters
sip | The SIP setting for the room
notifying | The notifying policy for the room

 **Table 3-2 Supported Media Formats**
Name|Type
-----|-----
opus | audio
isac_16000 | audio
isac_32000 | audio
g722_16000_1 | audio
pcma | audio
pcmu | audio
aac | audio
ac3 | audio
nellymoser | audio
ilbc | audio
h264 | video
h265 | video
vp8 | video
vp9 | video

> **Note**: When video format is "h264", for decoding, either software or hardware version OWT server supports all profiles. For encoding, software version OWT server with openh264 supports constrained-baseline profile and hardware version supports profiles including constrained-baseline, baseline, main, high.

 **Table 3-3 Default bitrate for typical resolutions (30fps)**
|Resolution|Default bitrate(kbps)|
|------|------|
|352x288 (cif)|442|
|176x144 (qcif)|132|
|320x240 (sif)|400|
|640x480 (vga)|800|
|800x600 (svga)|1137|
|1024x768 (xga)|1736|
|480x320 (hvga)|533|
|1280x720 (hd720p)|2000|
|1920x1080 (hd1080p)|4000|
|3840x2160 (uhd_4k)|16000|

The actual default bitrate will be affacted by resolution, framerate, motion factor, see the Table 3-3 to get overall data.
If framerate or resolution is specified in the subscription without bitrate, default bitrate is used and the view bitrate setting won't work.

> **Note**: If base layout is set to 'void', user must input customized layout for the room, otherwise the video layout would be treated as invalid. Read 3.5.1 for details of customized layout. maxInput indicates the maximum number of video frame inputs for the video layout definition.

### 3.5.1 Customized video layout {#Conferencesection3_5_1}
The OWT server supports the mixed video layout configuration which is similar as RFC5707 Media Server Markup Language (MSML).
A valid customized video layout should be an array of video layout definition.
The following example shows the details:

**Figure 3-1. Example Layout**
![Example layout](./pic/layout.jpg)

~~~~~~{.js}
// Video layout for the case of 1 input or 6 inputs
[
  {
    "region": [
      {
        "id": "1",
        "shape": "rectangle",
        "area": {
            "left": "0",
            "top": "0",
            "width": "1",
            "height": "1"
        }
      }
    ]
  },
  {
    "region": [
      {
        "id": "1",
        "shape": "rectangle",
        "area": {
            "left": "0",
            "top": "0",
            "width": "2/3",
            "height": "2/3"
        }
      },
      {
        "id": "2",
        "shape": "rectangle",
        "area": {
            "left": "2/3",
            "top": "0",
            "width": "1/3",
            "height": "1/3"
        }
      },
      {
        "id": "3",
        "shape": "rectangle",
        "area": {
            "left": "2/3",
            "top": "1/3",
            "width": "1/3",
            "height": "1/3"
        }
      },
      {
        "id": "4",
        "shape": "rectangle",
        "area": {
            "left": "2/3",
            "top": "2/3",
            "width": "1/3",
            "height": "1/3"
        }
      },
      {
        "id": "5",
        "shape": "rectangle",
        "area": {
            "left": "1/3",
            "top": "2/3",
            "width": "1/3",
            "height": "1/3"
        }
      },
      {
        "id": "6",
        "shape": "rectangle",
        "area": {
            "left": "0",
            "top": "2/3",
            "width": "1/3",
            "height": "1/3"
        }
      }
    ]
  }
]
~~~~~~
Each "region" defines video panes that are used to display participant video streams.

Regions are rendered on top of the root mixed stream. "id" is the identifier for each video layout region.

The size of a region is specified relative to the size of the root mixed stream using the "width" and "height" attributes.

Regions are located on the root window based on the value of the position attributes "top" and "left".  These attributes define the position of the top left corner of the region as an offset from the top left corner of the root mixed stream, which is a percent of the vertical or horizontal dimension of the root mixed stream.

### 3.5.2 Enable SIP connectivity {#Conferencesection3_5_2}
The OWT server supports connection from SIP clients. Before setting up SIP connectivity for rooms, make sure SIP server (like Kamailio) and related SIP user accounts are available. The SIP settings can be enabled through SDK or management console. On the console page, find the room that needs interaction with SIP clients and click the related "Room Detail" field. Then find the SIP setting fields in sub-section "sip".

The meaning of each field is listed below:

        sipServer: The SIP server's hostname or IP address.
        username: The user name registered in the above SIP server.
        password: The user's password.

After the SIP settings have been done, click the "Apply" button at the right side of the Room row to let it take effect. If the "Update Room Success" message shows up and the SIP related information is correct, then SIP clients should be able to join this room via the registered SIP server.

## 3.6 Cluster Worker Scheduling Policy Introduction {#Conferencesection3_6}
All workers including portals, conference-agents, webrtc-agents, streaming-agents, recording-agents, audio-agents, video-agents, sip-agents in the cluster are scheduled by the cluster-manager with respect to the configured scheduling strategies in cluster_manager/cluster_manager.toml.  For example, the configuration item "portal = last-used" means the scheduling policy of workers with purposes of "portal" is set to "last-used". The following built-in scheduling strategies are provided:
1. last-used: If more than 1 worker with the specified purpose is alive and available, the last used one will be scheduled.
2. least-used: If more than 1 worker with the specified purpose is alive and available, the one with the least work-load will be scheduled.
3. most-used: If more than 1 worker with the specified purpose is alive and available, the one with the heavist work-load will be scheduled.
4. round-robin: If more than 1 worker with the specified purpose is alive and available, they will be scheduled one by one circularly.
5. randomly-pick: If more than 1 worker with the specified purpose is alive and available, they will be scheduled randomly.

For portal and webrtc-agent workers, the "isp" and "region" preferences can be specified in portal and webrtc-agent configurations. Pass the preferred "isp" and "region" when creating a token, then cluster-manager will only schedule the corresponding worker nodes to the client requests that match "isp" and "region" preferences. The portal and webrtc-agent will serve all the isp and region if corresponding configuration items under "capacity" are set to empty lists. If no preferences are specified for clients when creating token, then only those portal and webrtc-agent with empty isp and region setting will serve them.

## 3.7 Runtime Configuration {#Conferencesection3_7}
Only super service user can access runtime configuration. Current management console implementation just provides the OWT server cluster runtime configuration viewer.

# 4 OWT Sample Application Server User Guide  {#Conferencesection4}
## 4.1 Introduction {#Conferencesection4_1}
The OWT sample application server is a Web application demo that shows how to host audio/video conference services powered by the Open WebRTC Toolkit. The sample application server is based on OWT runtime components. Refer to [Section 2](#Conferencesection2) of this guide, for system requirements and launch/stop instructions.

The source code of the sample application is in Release-<Version>/extras/basic_example/.

This section explains how to start a conference and then connect to a conference using different qualifiers, such as a specific video resolution.

## 4.2 Start a Conference through the OWT Sample Application Server {#Conferencesection4_2}
These general steps show how to start a conference:

1. Start up the OWT server components.
2. Launch your Google Chrome* browser from the client machine.
3. Connect to the OWT sample application server at: https://XXXXX:3004. Replace XXXXX with the IP address or machine name of the OWT sample application server.
> **Note**: Latest Chrome browser versions from v47 force https access on WebRTC applications. You will get SSL warning page with default certificates, replace them with your own trusted ones.
4. Start your conference with this default room created by the sample application server.

### 4.2.1 Connect to an OWT server conference with specific room {#Conferencesection4_2_1}
You can connect to a particular conference room. To do this, simply specify your room ID via a query string in your URL: room.
For example, connect to the OWT sample application server XXXXX with the following URL:

        https://XXXXX:3004/?room=some_particular_room_id
This will direct the conference connection to the room with the ID some_particular_room_id.
### 4.2.2 Connect to an OWT server conference to subscribe forward streams {#Conferencesection4_2_2}
Since OWT conference room can now produce both forward streams and mixed stream at the same time, including the screen sharing stream, the client is able to subscribe specified stream(s) by a query string in your URL: forward. The default value for the key word is false.

For example, to subscribe forward stream instead of mixed stream from OWT server, connect to the OWT sample application server XXXXX with the following URL:

        https://XXXXX:3004/?forward=true

### 4.2.3 Connect to an OWT conference with an RTSP input {#Conferencesection4_2_3}
The OWT conference supports external stream input from devices that support RTSP protocol, like IP Camera.
For example, connect to the OWT sample application server XXXXX with the following URL:

        https://XXXXX:3004/?url=rtsp_stream_url

# 5 Peer Server {#Conferencesection5}
## 5.1 Introduction {#Conferencesection5_1}
The peer server is the default signaling server of the Open-WebRTC-Toolkit. The peer server provides the ability to exchange WebRTC signaling messages over Socket.IO between different clients.

**Figure 5-1. Peer Server Framework**
<img src="./pic/framework.png" alt="Framework" style="width: 600px;">

## 5.2 Installation requirements {#Conferencesection5_2}
The installation requirements for the peer server are listed in Table 5-1 and 5-2.

**Table 5-1. Installation requirements**
Component name | OS version
----|-----
Peer server | Ubuntu 18.04 LTS, CentOS* 7.6/7.4

> **Note**: The peer server has been fully tested on Ubuntu14.04 LTS,64-bit.
**Table 5-2. Peer Server Dependencies**
Name | Version | Remarks
-----|----|----
Node.js | 8.15.0 | Website: http://nodejs.org/
Node modules | Specified | N/A

Regarding Node.js*, make sure it's installed in your system prior to installing the Peer Server. We recommend version 8.15.0. Refer to http://nodejs.org/ for installation details.
## 5.3 Installation {#Conferencesection5_3}
On the server machine, unpack the peer server release package, and install node modules

        tar –zxvf CS_WebRTC_Conference_Server_Peer.v<Version>.tgz
        cd PeerServer-Release-<Version>
        npm install

The default http port is 8095, and the default secured port is 8096.  Those default values can be modified in the file config.json.
## 5.4 Use your own certificate  {#Conferencesection5_4}
If you want to use a secured socket.io connection, you should use your own certificate for the service. A default certificate is stored in cert with two files: cert.pem and key.pem. Replace these files with your own certificate and key files.
## 5.5 Launch the peer server {#Conferencesection5_5}
Run the following commands to launch the peer server:

        cd PeerServer-Release-<Version>
        node peerserver.js

## 5.6 Stop the peer server {#Conferencesection5_6}
Run the **kill** command directly from the terminal to stop the peer server.

# 6 Appendix {#Conferencesection6}
## 6.1 Media Analytics in OWT Server {#Conferencesection6_1}
### 6.1.1 Introduction {#Conferencesection6_1_1}
OWT server provides media analytics functionality via REST API. The media analytics plugins allows you performing analytics on any stream in OWT server, generating new streams and/or emitting events during analytics. The proposed usage scenario for real-time media analytics includes but is not limited to movement/object detection in surveillance and remote health care, customer/audience analyzing in retail and remote education, etc.

For usage of media analytics REST API, refer to the REST API document.

### 6.1.2 Building Existing Plugins {#Conferencesection6_1_2}
A few sample plugins are shipped with OWT server. After you build and install OWT server, the source of those analytics plugins will be placed under analytics_agent/plugins/ directory.

Before you build those plugins, you need to install Intel Distribution of OpenVINO 2018 R5 from [https://software.intel.com/en-us/openvino-toolkit/](https://software.intel.com/en-us/openvino-toolkit/). After installation, make sure you go to /opt/intel/computer_vision_sdk/install_dependencies/ directory, and run:
    sudo -E ./install_NEO_OCL_driver.sh

Intel Distribution of OpenVINO 2018 R5 will require Intel Core 6th to 8th Generation with Intel HD graphics or Iris(Pro) graphics for inferencing on GPU with OpenCL as the backend. The supported OSes are Ubuntu 16.04 and higher, or CentOS 7.4 or higher. Make sure your hardware/software configuration is correct before building and deploying the sample plugins.

To build the plugins, simply go to analytics_agent/plugins/sample directory, and run:
    ./build_samples.sh

Copy all files under build/intel64/Release/lib/ to analytics_agent/lib/ directory, or to the libpath as specified in agent.toml in analytics_agent.

### 6.1.3 Using Pre-built Plugins {#Conferencesection6_1_3}
The Server provides 4 plugins as source code which can be built by your own. To verify them, make sure you run ```source /opt/intel/computer_vision_sdk/bin/setupvars.sh``` before starting up OWT server.

#### 6.1.3.1 Face Detection Plugin {#Conferencesection6_1_3_1}
Identified by GUID b849f44bee074b08bf3e627f3fc927c7. This plugin provides the capability of finding faces in current analyzed stream and annotates it with a rectangle boarder on the face.
#### 6.1.3.2 Face Recognition Plugin {#Conferencesection6_1_3_2}
Identified by GUID 3f932ff2a80341faa0a73ebb3bcfb85d. This plugin provides the capability of identifying people's name in current stream and annotates them with a rectangle on the face, and also list the name and the confidence of the recognition result.
To add new people for recognition, here are the steps:
1. Take at least 3 pictures of one person and place them under the raw_photos directory with sub-directory name that identifies that person's name(no space in the directory name). The raw_photos should be under the same directory with the pre-process tool.
2. Run the pre-process tool to process the raw photos (which is also built when you build the samples). Append the path of libcpu_extension.so you built to LD_LIBRARY_PATH before you run the tool. Put the output vectors.txt under analytics_agent direcotry of OWT server.
#### 6.1.3.3 Smart Class Room Plugin {#Conferencesection6_1_3_3}
Identified by GUID 10c213f3d55249718d3bd44712488502. This plugin provides the capability of recognizing the gestures in the stream and annotates the gestures accordingly.
#### 6.1.3.4 Dummy Plugin {#Conferencesection6_1_3_4}
Identified by GUID dc51138a8284436f873418a21ba8cfa7. This plugin simply modifies part of the stream to demonstrate the working process of plugins.

### 6.1.4 Create and Deploy Your Own Media Analytics Plugin {#Conferencesection6_1_4}
OWT server allows you creating your own media analytics plugins and deploy them to OWT server. Refer to the analytics_agent/plugins/include/plugin.h for more detailed API interface that each media analytics plugin needs to implement.

#### 6.1.4.1 Create Plugin {#Conferencesection6_1_4_1}
Your plugin class implementation must inherit from rvaPlugin interface as defined in analytics_agent/plugins/include/plugin.h. Besides the plugin class implementation, it is required to include the DECLARE_PLUGIN(ClassName) macro to export your plugin implementation.
#### 6.1.4.2 Deploy Your Plugin {#Conferencesection6_1_4_2}
To deploy a plugin to OWT Server, you will need to generate a new GUID for your plugin. After that, copy your plugin .so files to analytics_agent/lib, or to the libpath as specified by agent.toml of analtyics agent. Also you need to add an entry into the plugin.cfg file under analytics_agent with the GUID you generated, for example:
	[c842f499aa093c27cf1e328f2fc987c7]
	description = 'my own plugin'
	pluginversion = 1
	apiversion =400
	name = 'libsomeplugin.so' # full name of the plugin library
	libpath = 'pluginlibs/' # relative to analytics_agent directory
	configpath = 'pluginlibs/' # relative to analytics agent directory
	messaging = true       # set to false if your plugin does not send notification
	inputfourcc = 'I420'   # must be I420 for current version
	outputfourcc = 'I420'  # set to "" if your plugin will not republish analyzed stream to OWT server.

Restart analytics agent and your plugin will be added to OWT server.
