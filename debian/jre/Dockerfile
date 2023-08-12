FROM debian:11

MAINTAINER 852203465@qq.com

ENV JAVA_TAR_VERSION=8u371
ENV JAVA_VERSON=1.8.0_371
ENV JAVA_HOME=/usr/local/java
ENV CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/lib:$CLASSPATH
ENV PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/bin

# 设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

WORKDIR /home

ADD ./jre-${JAVA_TAR_VERSION}-linux-x64.tar.gz /home
ADD ./Fonts.tar.gz /usr/share/fonts/
COPY locale.gen /etc/locale.gen

RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list  \
    && sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && apt-get update  \
    && apt-get install -y wget curl net-tools locales fontconfig \
    && chmod 777 /usr/share/fonts \
    && locale-gen \
    && fc-cache -fv \
    && rm -rf /var/lib/apt/lists/*

# 安装JDK
RUN mkdir -p /usr/local/java \
    && mv /home/jre${JAVA_VERSON}/* /usr/local/java \
    && rm -rf /home/jre${JAVA_VERSON}

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