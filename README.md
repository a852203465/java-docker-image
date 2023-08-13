# JAVA(LTS) Docker 镜像制作
对JAVA镜像制作，并增加字体，部分中文语言包
 - 支持en_US zh_CN zh_HK zh_SG zh_TW zu_ZA

## 1.支持版本
 - JAVA8
 - JAVA11
 - JAVA17

## 2.镜像中的依赖文件下载地址
```text
链接：https://pan.baidu.com/s/1Rm1B8SX28VTwVZMhtEv5tg 
提取码：1234
```

## 3.项目分支说明

* Active branches:
    * `master` - Latest Java, currently version 17
* Inactive branches:
    * `java17` - Java 17
    * `java11` - Java 11
    * `java8` - Java 8

- alpine: 以`alpine:15`为基础镜像制作
- debian: 以`debian:11`为基础镜像制作
- centos: 以`centos:7`为基础镜像制作
- ubuntu: 以`ubuntu:22.04`为基础镜像制作
- -jdk: 包含JDK完整环境
- -jre: 仅JRE运行环境

## 4.关于Alpine Linux镜像启动JAVA应用
此时JAVA应用的PID为1，此时无法通过jmap和jinfo查看JVM相关信息
### 4.1背景
- PID 1~5是Linux的特殊进程
- PID为1的进程结束后，容器会随之结束，这样可以通过宿主机监控，保证及时检测到容器的异常退出
- docker正常关闭时，PID为1的进程能够接收到关闭信号，从而保证实现优雅关闭

* PID 1~5是Linux的特殊进程
  * pid=1: init进程，系统启动的第一个用户级进程，是其他所有进程的父进程，引导用户空间服务
  * pid=2: kthreadd，内核线程管理
  * pid=3: migration，用于进程在不同CPU间迁移
  * pid=4: ksoftirqd，内核中软中断守护线程，用于系统空闲时定时处理软中断事务
  * pid=5: watchdog，看门狗进程，用于监听内核异常，当系统出现宕机时，可利用watchdog记录宕机时堆栈信息

### 4.2解决方法
- 开子线程方式
- 使用 `tini`

#### 4.2.1使用 `tini`
alpine镜像中已安装, 直接使用即可
```dockerfile
FROM registry.cn-shenzhen.aliyuncs.com/a852203465/java:jdk8-alpine-1.0
COPY ./target/app.jar app.jar
EXPOSE 2023
ENTRYPOINT ["tini", "java", "-jar", "app.jar", "&"]
```

```dockerfile
FROM registry.cn-shenzhen.aliyuncs.com/a852203465/java:jdk11-alpine-1.0
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone
ADD ./*.jar app.jar
ENTRYPOINT ["tini", "/bin/sh", "-c", "set -e && java -Xms2048m -Xmx4096m -Djava.security.egd=file:/dev/./urandom -Dfile.encoding=utf-8 -jar /app.jar"]
```

#### 4.2.2开子线程方式

将执行命令写入脚本中,然后sh 执行脚本, 创建`run.sh`脚本, 然后修改dockerfile
```shell
  #!/bin/sh
  java $JAVA_OPTS   -jar tmp/$app.jar
```

```dockerfile
COPY ./target/$app.jar /tmp
ENTRYPOINT [ "sh","-c","/tmp/run.sh"]
```

## 5.Docker Tags

`registry.cn-shenzhen.aliyuncs.com/a852203465/java` provides several tagged images

* JAVA 8
  * **jdk8-alpine-1.0**: `Oracle Java SE Development Kit 8 update 381` on top of **Alpine Linux**
  * **jdk8-centos-1.0**: `Oracle Java SE Development Kit 8 update 381` on top of **CentOS**
  * **jdk8-debian-1.0**: `Oracle Java SE Development Kit 8 update 381` on top of **Debian**
  * **jdk8-ubuntu-1.0**: `Oracle Java SE Development Kit 8 update 381` on top of **Ubuntu**
  * **jre8-alpine-1.0**: `Oracle Java SE Runtime Environment 8 update 381` on top of **Alpine Linux**
  * **jre8-centos-1.0**: `Oracle Java SE Runtime Environment 8 update 381` on top of **CentOS**
  * **jre8-debian-1.0**: `Oracle Java SE Runtime Environment 8 update 381` on top of **Debian**
  * **jre8-ubuntu-1.0**: `Oracle Java SE Runtime Environment 8 update 381` on top of **Ubuntu**

* JAVA 11
  * **jdk11-alpine-1.0**: `Oracle Java SE Development Kit 11.0.20` on top of **Alpine Linux**
  * **jdk11-centos-1.0**: `Oracle Java SE Development Kit 11.0.20` on top of **CentOS**
  * **jdk11-debian-1.0**: `Oracle Java SE Development Kit 11.0.20` on top of **Debian**
  * **jdk11-ubuntu-1.0**: `Oracle Java SE Development Kit 11.0.20` on top of **Ubuntu**

* JAVA 17
  * **jdk17-alpine-1.0**: `Oracle Java SE Development Kit 17.0.8` on top of **Alpine Linux**
  * **jdk17-centos-1.0**: `Oracle Java SE Development Kit 17.0.8` on top of **CentOS**
  * **jdk17-debian-1.0**: `Oracle Java SE Development Kit 17.0.8` on top of **Debian**
  * **jdk17-ubuntu-1.0**: `Oracle Java SE Development Kit 17.0.8` on top of **Ubuntu**













































































