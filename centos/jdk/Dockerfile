FROM centos:7

MAINTAINER 852203465@qq.com

ENV JAVA_VERSON=11.0.20
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

RUN curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo \
    && sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo \
    && yum -y upgrade \
    && yum -y update \
    && yum -y install -y wget net-tools locales fontconfig

# 安装JDK
RUN mkdir -p /usr/local/java \
    && mv /home/jdk-${JAVA_VERSON}/* /usr/local/java \
    && rm -rf /home/jdk-${JAVA_VERSON}

# 安装字体,语言
RUN yum -y reinstall kde-l10n-Chinese glibc-common \
    && cat locale.md | tr -d '\r' | xargs -i localedef -c -f UTF-8 -i {} {}.UTF-8  \
    && echo 'LANG="zh_CN.UTF-8"' > /etc/locale.conf \
    && rm -rf locale.md \
    && chmod 777 /usr/share/fonts \
    && fc-cache -fv

# 清理
RUN  yum clean all  \
    && rm -rf /tmp/* /var/cache/yum/* /var/tmp/*

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






















