NOTE: 文章内容按照时间`pr`合入的时间线进行排列。

仓库地址：[gcs-back-end](https://github.com/CMIPT/gcs-back-end)

# Add docker creator and format action
`pr`的链接：[gcs-pull-1](https://github.com/CMIPT/gcs-back-end/pull/1)

## Google Java Style Format
本次`pr`添加了`docker`的创建和格式化`action`。该`action`能够在创建合入`master`和`develop`的
`pr`时，自动使用`Google Java Style Format`对`Java`代码进行格式化，并且自动创建一个新的`commit`提交
格式化后的代码。

创建这个`git action`的主要目的是为了保证整个团队编码风格的一致性。

Issues: 
- [ ] 这个`action`目前还有一些问题，比如新创建的提交依然会触发一次该`action`。

NOTE: `git action`的代码中出现了`secrets.PAT`，该字段需要在仓库的设置部分自行进行设置，值的内容是一
个`token`该`token`具有仓库的写权限，这样自动提交的时候才能保证能够成功提交。而`token`的创建方法是在
个人的`github`账号中。

在`github`中创建`token`：

![](https://raw.githubusercontent.com/Kaiser-Yang/image-hosting-site/main/20240421-20250421/20240801102637.png)

如何在仓库的`secrets`中添加`token`：

![](https://raw.githubusercontent.com/Kaiser-Yang/image-hosting-site/main/20240421-20250421/20240801102200.png)

## Docker Creator
本次`pr`中还添加了一个第三方依赖，用于创建`docker`镜像。这个镜像能够自动创建一个`docker`镜像并安装
一些基础的工具，例如`ssh`等。

第三方依赖的地址：[docker-creator](https://github.com/CMIPT/docker-script.git)

# Initialize the project
`pr`的链接：[gcs-pull-2](https://github.com/CMIPT/gcs-back-end/pull/2)

本次`pr`是从[spring-io](https://start.spring.io/)上创建了一个`Spring Boot`的项目，然后将其解压到了
仓库中。

TODO:
- [ ] 添加`spring`的基本使用介绍

# Build the initial database script
`pr`的链接：[gcs-pull-3](https://github.com/CMIPT/gcs-back-end/pull/3)

# Provide Python scripts to process Json files
`pr`的链接：[gcs-pull-6](https://github.com/CMIPT/gcs-back-end/pull/6)

# Add spring-doc for restful api
`pr`的链接：[gcs-pull-7](https://github.com/CMIPT/gcs-back-end/pull/7)

本次的`pr`主要是添加了`spring-doc`的依赖，用于生成`restful api`的文档。

集成文档的时候开始在尝试使用`spring-fox`和`spring-swagger`。结果发现和`spring3.x`的版本不兼容，然后
搜索资料发现了`spring-doc`这个依赖，然后就使用了这个依赖。`

TODO:
- [ ] 添加`spring-doc`的使用介绍

# Finish part of the deploy script
`pr`的链接：[gcs-pull-9](https://github.com/CMIPT/gcs-back-end/pull/9)

创建这个脚本的目的是希望能够仅仅通过编写一个`json`的配置文件，就能够完成整个项目的部署。这个`pr`完成
了部分的功能，还有一些功能没有完成。

主脚本是一个`bash`脚本，在`bash`脚本中会自动安装`python`等依赖，然后调用`python`脚本来完成部署。部署
的主要逻辑是通过读取`json`文件，然后根据配置文件完成一系列处理，最后将`jar`通过`Sys V init`注册成
一个服务。

`Sys V init`创建服务的方式非常简单，只需要在`/etc/init.d/`目录下放置一个可执行文件，然后即可通过
`service xxx start`来启动服务。在本次的`pr`中，直接创建了一个软连接连接到了打包出来的`jar`文件，但是
在后续的使用过程中发现这样做存在问题。

在本次的提交中，我发现`junit`可以使用`spring-boot-test`进行替代，于是将之前的`junit`的依赖替换成了
`spring-boot-test`，并且删除了之前添加的`log4j`依赖（后续可以自己设置`spring-boot`的日志系统）。

下面介绍一下几个脚本有什么作用：
* `deploy_ubuntu.sh`：这个脚本是主脚本，用于在`ubuntu`系统上部署项目，这个脚本再安装一些依赖后便将控制权交给`script/deploy_helper.py`。
* `script/deploy_helper.py`：这个脚本读取`json`配置文件，然后根据配置文件完成一系列操作，最后将`jar`注册成一个服务。
* `script/get_jar_position.sh`：获取`jar`的位置，这个脚本会在`deploy_helper.py`中被调用，用于获取`mvn package`输出位置。

Issues: 
- [x] 这个脚本在某些环境使用的时候出现了问题，详见[gcs-issues-14](https://github.com/CMIPT/gcs-back-end/issues/14)
- [ ] 错误的添加使用了`spring-boot-test`导致添加了`spring-boot-starter-webflux`依赖，这个依赖目前应该是不用的。

# Add MIT license and developers info
`pr`的链接：[gcs-pull-13](https://github.com/CMIPT/gcs-back-end/pull/13)

本次`pr`主要是添加了`MIT`的开源协议，以及开发者的信息。

# Substitute the Sys V init with systemd
`pr`的链接：[gcs-pull-15](https://github.com/CMIPT/gcs-back-end/pull/15)

本次`pr`主要是将`Sys V init`部署服务的方式替换成了`systemd`。这样做是因为发现现代的`Linux`系统更多地
使用`systemd`来管理服务，且`systemd`的启动速度等更加优秀，配置更加简单。这次的提交修复了
[gcs-issues-14](https://github.com/CMIPT/gcs-back-end/issues/14)。

这次的提交中还增加了`config_default.json`文件，`config_default.json`文件存储默认的配置，当一个配置项
没有被用户重写的时候会使用默认的配置。

同时新增了一个`clean_ubuntu.sh`脚本用于清空创建的服务等。

下面简单介绍一下`systemd`的使用方法。

要使用`systemd`来管理服务，首先需要创建一个`service`文件，然后将这个文件放置在`/etc/systemd/system/`
目录下。`service`文件的内容大致如下：

```shell
[Unit]
Description=Git server center back-end service
After=network.target

[Service]
PIDFile=/var/run/gcs.pid
User=gcs
WorkingDirectory=/opt/gcs
Restart=always
RestartSec=5
ExecStart=/usr/bin/java -jar /opt/gcs/gcs.jar

[Install]
WantedBy=multi-user.target
```

上面的大部分字段是不用解释的，这里给出一些注意事项：
* 路径全部使用绝对路径
* `systemctl enable gcs`表示将服务设置为开机启动，但是如果当前的服务并没有启动，那么这个命令并不会启动服务
* `systemctl start gcs`表示启动当前的服务，这与`gcs`是否被设置为开机启动无关
* `systemctl disable gcs`和`systemctl stop gcs`的关系与上述类似

NOTE: 简单解释一下`systemctl enable gcs`和`systemctl disable gcs`的原理。两者只做了一件事情：
根据`[Install]`部分的内容创建或者删除软连接，例如上面的例子中，`systemctl enable gcs`会在
`/etc/systemd/system/multi-user.target.wants/`目录下创建一个`gcs.service`的软连接，而
`systemctl disable gcs`会删除这个软连接。这个软连接的作用是在`multi-user.target`启动的时候启动`gcs`
而`Linux`系统启动的时候会启动`multi-user.target`，所以`gcs`也能在开机的时候自动启动。