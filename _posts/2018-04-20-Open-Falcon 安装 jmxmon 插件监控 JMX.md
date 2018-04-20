---
title: Open-Falcon 安装 jmxmon 插件监控 JMX
date: 2018-04-20 12:26:25
categories:
- 运维
tags:
- 运维
- Open-Falcon
- jmxmon
- JMX
---
**安装步骤**

1. 安装并启动 Open-Falcon Agent
2. 下载并解压编译好的 [Release](https://github.com/toomanyopenfiles/jmxmon/releases/latest) 包到目标安装目录下
3. `cp conf.example.properties conf.properties`
4. 修改 conf.properties 配置文件，一般情况下只需要将 jmx.ports 的端口号配置上就可以了
5. `sh control start`
6. `sh control tail` 查看日志，或者 `cat var/app.log` 以确认程序是否正常启动

**参考目录**

Agent 安装目录：`/home/src/github.com/open-falcon/agent/`
jmxmon 解压后的目录：`/home/src/github.com/open-falcon/agent/jmxmon-v0.0.2`

**启动 Java 程序配置 JMX Port 参考**

```shell
java -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=9996 -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -jar demo.jar
```

所以 `jmx.ports = 9996`

**conf.properties**

```shell
# the working dir
workDir=./

# localhost jmx ports, split by comma
jmx.ports=9996

# agent port url
agent.posturl=http://localhost:1988/v1/push
```

**Agent port**

可以到 Agent 安装目录下的 cfg.json 查看。

`vi /home/src/github.com/open-falcon/agent/cfg.json`

```shell
  "http": {
    "enabled": true,
    "listen": ":1988",
    "backdoor": false
  },
```