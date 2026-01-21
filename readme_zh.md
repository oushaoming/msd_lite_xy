## 中文说明

### 项目简介
msd_lite 是一款轻量级的多流守护进程，用于通过 HTTP 在网络上组织 IP TV 流媒体服务。它支持接收 UDP 组播流（包括 RTP 流），并将其转换为 HTTP 流提供给客户端。

### 功能特性
* 开源软件，基于 BSD 许可证
* 稳定可靠，运行时无线程死锁
* 支持接收 UDP 组播流，包括 RTP 流
* 始终启用发送零拷贝（Zero Copy on Send）
* 无需轮询即可向客户端发送数据
* 无需分析 MPEG2-TS 流，智能地向新客户端发送 MPEG2-TS 头部

### 编译安装
```bash
sudo apt-get install build-essential git cmake fakeroot
git clone --recursive https://github.com/rozhuk-im/msd_lite.git
cd msd_lite
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_VERBOSE_MAKEFILE=true ..
make -j 8
```

### 运行测试
```bash
mkdir -p build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DENABLE_TESTS=1 ..
cmake --build . --config Release -j 16
ctest -C Release --output-on-failure -j 16
```

### 使用方法
```
msd_lite [-d] [-v] [-c file]
       [-p PID file] [-u uid|usr -g gid|grp]
 -h           显示帮助信息
 -d           以守护进程模式运行
 -c file      指定配置文件
 -p PID file  存储 PID 的文件路径
 -u uid|user  切换到指定的用户
 -g gid|group 切换到指定的组
 -v           开启详细日志
```

### 配置设置
将 %%ETCDIR%%/msd_lite.conf.sample 复制到 %%ETCDIR%%/msd_lite.conf，然后将 lan0 替换为您的网络接口名称。根据需要添加更多配置部分。如果不需要 IPv4 或 IPv6 支持，可以删除相应的配置行。

在 /etc/rc.conf 中添加：
```
msd_lite_enable="YES"
```

运行服务：
```
service msd_lite restart
```
