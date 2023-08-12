FROM ubuntu:22.04

MAINTAINER 852203465@qq.com

ENV JAVA_VERSON=17.0.8
ENV JAVA_HOME=/usr/local/java
ENV CLASSPATH=.:$JAVA_HOME/lib
ENV PATH=$PATH:$JAVA_HOME/bin

# 设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

WORKDIR /home

ADD ./jdk-${JAVA_VERSON}_linux-x64_bin.tar.gz /home
ADD ./Fonts.tar.gz /usr/share/fonts/
COPY ./locale.md /home/locale.md

RUN sed -i s/archive.ubuntu.com/mirrors.aliyun.com/g /etc/apt/sources.list \
    && sed -i s/security.ubuntu.com/mirrors.aliyun.com/g /etc/apt/sources.list \
    && apt-get -y upgrade \
    && apt-get -y update \
    && apt-get -y install -y wget net-tools locales fontconfig

# 安装JDK
RUN mkdir -p /usr/local/java \
    && mv /home/jdk-${JAVA_VERSON}/* /usr/local/java \
    && rm -rf /home/jdk-${JAVA_VERSON}

# 安装字体,语言
RUN cat locale.md | tr -d '\r' | xargs -i localedef -c -f UTF-8 -i {} {}.UTF-8  \
    && rm -rf locale.md \
    && chmod 777 /usr/share/fonts \
    && fc-cache -fv

# 清理
RUN  apt-get clean all  \
    && rm -rf /tmp/* /var/tmp/*

# 指定语言
ENV LANG=zh_CN.UTF-8
ENV LANGUAGE=zh_CN.UTF-8
ENV LC_ALL=zh_CN.UTF-8

# 检查环境
RUN java -version \
    && fc-list \
    && locale -a

WORKDIR /

CMD ["java","-version"]






















