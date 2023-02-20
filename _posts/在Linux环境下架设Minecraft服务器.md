---
title: 在Linux环境下架设Minecraft服务器
tags:
  - Minecraft
  - 游戏
  - 服务器
  - Linux
categories: 游戏开服
date: 2022-07-27 23:25:01
---


![Minecraft 徽标](https://www.minecraft.net/content/dam/games/minecraft/logos/logo-minecraft.svg)

<!--more-->

## 服务器架设

### 服务器准备

- 放通所有UDP/TCP端口（根据个人需求，可以选择放通指定端口）

### 为服务端添加一个用户

直接使用root账户进行开服会存在一定风险，因此建议新建用户进行权限隔离。

```shell
#新建用户minecraft
adduser minecraft
#将用户minecraft添加到sudo组，以便在minecraft用户下使用sudo
sudo adduser minecraft sudo
#切换用户minecraft
su minecraft
```

### 安装相关软件

```shell
#更新apt软件列表
sudo apt update
#安装Java
#可用java -version来查看服务器已经安装的版本
sudo apt install openjdk-17-jdk-headless
#安装screen（部分服务器内置）
sudo apt install screen
```

### 安装Minecraft服务端

Minecraft有很多服务端，在这里我们选择官方提供的原生服务端，你可以在[这里](https://www.minecraft.net/zh-hans/download/server)找到它的页面。

1. 在`~`下新建目录`mcserver`

    ```shell
    cd ~
    mkdir mcserver
    cd mcserver
    ```

2. 下载`minecraft_server.1.19.jar`

    ```shell
    wget https://launcher.mojang.com/v1/objects/e00c4052dac1d59a1188b2aa9d5a87113aaf1122/server.jar
    ```

3. 运行服务端

    ```shell
    java -Xms1024M -Xmx2048M -jar server.jar nogui
    ```

    > 如果想使用图形用户界面启动服务器，您可以省略`nogui`部分。
    >
    > 初次启动会提示要求同意EULA，只需要将jar目录下的`eula.txt`中的`eula=false`改成`eula=true`即可。

### 配置Minecraft服务端

`server.properties`用于配置服务器，其编写可以参考附录。

## 配置启动关闭脚本

直接使用`java`命令是最直接的，但这样的话进程会过于依赖Terminal，导致我们关闭Terminal窗口后，进程也会被Kill，这未免有些不便。

考虑到这种情况，我们可以编写一个脚本，并利用screen实现进程与Terminal的解耦。

- 启动脚本 (`launch.sh`)

    ```sh
    #!/bin/sh

    screen -dmS mc java -Xms1024M -Xmx2048M -jar /home/minecraft/mcserver/server.jar
    ```

- 关闭脚本 (`stop.sh`)

    ```sh
    #!/bin/sh

    screen -dr mc -X stuff "say 服务器将在10S后关闭！\n"
    sleep 10
    screen -dr mc -X stuff "stop\n"
    ```

- 重启脚本 (`restart.sh`)

    ```shell
    #!/bin/sh
    
    screen -dr mc -X stuff "say 服务器将在10S后例行重启！\n"
    sleep 10
    ./stop.sh
    sleep 20
    ./launch.sh
    ```

- 赋予脚本权限

    ```
    chmod +x launch.sh
    chmod +x stop.sh
    chmod +x restart.sh
    ```

- 脚本的使用

    ```shell
    #启动服务器
    ./launch.sh
    #关闭服务器
	./stop.sh
	#重启服务器
	./restart.sh
	#恢复窗口
	screen -r mc
	```
	
	> 想要将服务端控制台放到后台运行，可以按`Ctrl`+`A`，然后再按`D`，即可将screen放到后台，此时关闭Terminal也不会Kill进程。

## 附录

### `server.properties`参考

```properties
#Minecraft server properties
#Wed Jul 27 21:38:29 CST 2022
enable-jmx-monitoring=false
rcon.port=25575
level-seed=
#默认游戏模式 0-生存 1-创造 2-冒险
gamemode=survival
#是否允许使用命令方块
enable-command-block=false
enable-query=false
generator-settings={}
enforce-secure-profile=false
level-name=world
motd=A Minecraft Server
query.port=25565
#是否允许PVP
pvp=true
#是否生成结构（村庄，女巫小屋等）
generate-structures=true
max-chained-neighbor-updates=1000000
#难度 0-和平 1-简单 2-普通 3-困难
difficulty=easy
#网络压缩阈值，较低节省网络资源，较高节省性能。默认256，推荐64-512内
network-compression-threshold=128
max-tick-time=60000
require-resource-pack=false
use-native-transport=true
#同时在线的最大人数
max-players=20
#是否开启在线验证（是否允许盗版玩家）
online-mode=false
enable-status=true
allow-flight=false
broadcast-rcon-to-ops=true
#可视范围 默认10 推荐6 如果有刷怪塔推荐8
view-distance=8
server-ip=
resource-pack-prompt=
allow-nether=true
#服务器端口
server-port=25565
enable-rcon=false
sync-chunk-writes=true
op-permission-level=4
prevent-proxy-connections=false
hide-online-players=false
#是否使用服务器资源包（需要填入URL链接）
resource-pack=
entity-broadcast-range-percentage=100
simulation-distance=10
rcon.password=
player-idle-timeout=0
#玩家进入时是否强制更改其游戏模式
force-gamemode=true
rate-limit=0
hardcore=false
#是否启用白名单，将从whitelist.json加载白名单
white-list=false
broadcast-console-to-ops=true
spawn-npcs=true
previews-chat=false
spawn-animals=true
function-permission-level=2
level-type=minecraft\:normal
text-filtering-config=
spawn-monsters=true
enforce-whitelist=false
spawn-protection=16
resource-pack-sha1=
max-world-size=29999984
```



## 参考资料

[教程/架设服务器 - Minecraft Wiki，最详细的我的世界百科 (fandom.com)](https://minecraft.fandom.com/zh/wiki/教程/架设服务器)

