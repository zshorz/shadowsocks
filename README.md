# shadowsocks
![](https://img.shields.io/github/last-commit/zshorz/shadowsocks) [![](https://travis-ci.org/zshorz/shadowsocks.svg?branch=master)](https://www.travis-ci.org/zshorz/shadowsocks/builds) [![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/zshorz/shadowsocks)](https://hub.docker.com/repository/docker/zshorz/shadowsocks/builds)

科学上网



## 使用docker

docker仓库地址：[https://hub.docker.com/r/zshorz/shadowsocks](https://hub.docker.com/r/zshorz/shadowsocks)

```shell
# 拉取
docker pull zshorz/shadowsocks
# 以daemon方式运行
docker run --name ss-server --rm -d -p 9999:9999/tcp -p 9999:9999/udp zshorz/shadowsocks:latest -p 9999 -k 123456 -uri -u
# 查看输出以便于得到 URI， 强烈建议映射端口时使用相同端口号，否则 URI 无效
docker logs ss-server 
```



## ss-server

构建

```shell
git clone https://github.com/zshorz/shadowsockets.git
cd shadowsockets/ss-server
go build -o ss-server.exe .
./ss-server.exe -c ../config.json

# 命令行指定参数 并创建配置文件的方式启动
./ss-server.exe -p 9999 -k 123456 -w -uri
```

参数说明

```shell
# docker下不使用配置文件，全部命令行指定
-version		# 打印版本后退出
-c filename		# 指定配置文件 docker不使用
-k passwd		# 服务器密码
-p serverPort	# 指定服务器端口
-t timeout		# 超时时间
-m method		# 加密方法
-core n			# 最大线程数
-d				# debug模式
-u				# udp relay
-w				# 命令行输入的配置写入配置文件 docker不使用
-uri			# 打印URI， 会自动获取公网ip
-domain domain/ip   # 如果指定domain,则在生成uri的时候阻止获取公网ip,用指定的domain/ip代替
-sanitize       # 在debug模式下，ip:port被处理为 x.x.x.x:zzzz
#-manager-address
```





## ss-local

构建

```shell
git clone https://github.com/zshorz/shadowsockets.git
cd shadowsockets/ss-local
go build -o ss-local.exe .
./ss-local.exe -c ../config.json

# 命令行指定参数 并创建配置文件的方式启动
./ss-local.exe -s 127.0.0.1 -p 9999 -k 123456 -b 1080 -w
# 通过 URI 启动
./ss-local.exe -b 1080 -u ss://YWVzLTI1N...
```

参数说明

```shell
# 每个参数都有默认值， 如果找不到配置文件，在运行目录生成名为 config.json 的配置文件
-version		# 打印版本后退出
-c filename		# 指定配置文件
-s server		# 指定服务器
-p serverPort	# 指定服务器端口
-k passwd		# 服务器密码
-b address		# 本地地址
-l port			# 本地端口
-m method		# 加密方法
-t timeout		# 超时时间
-d				# debug模式
-w				# 命令行输入的配置写入配置文件
-u				# 通过URI导入
```

## 关于
* 请保证go版本大于1.11，需要 go mod 支持
* 出于学习的目的，这是我个人维护的版本，fork自[shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)（代码已有较大变动）
* 你可以在[这里](https://github.com/shadowsocks)找到各个平台的客户端（ss-local目前只是用来调试，功能并不健全）
    * [ss-c#](https://github.com/shadowsocks/shadowsocks-windows/releases) for Windows
    * [ss-android](https://github.com/shadowsocks/shadowsocks-android/releases) for Android
    * 当然也可以使用ssr的客户端，ssr兼容ss
* 有关协议握手细节可以查看 [detail.md](./detail.md) 和 [rfc1928.txt](./rfc1928.txt)

