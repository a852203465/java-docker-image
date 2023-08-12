FROM alpine:3.15

MAINTAINER 852203465@qq.com

ENV GLIBC_VERSION=2.33-r0
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
COPY ./locale.md /home/locale.md
COPY ./glibc* /home
COPY ./sgerrand.rsa.pub /etc/apk/keys/

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories  \
    && apk update  \
    && apk --no-cache add ca-certificates wget curl net-tools tini \
    && apk add glibc-bin-${GLIBC_VERSION}.apk glibc-i18n-${GLIBC_VERSION}.apk glibc-${GLIBC_VERSION}.apk  \
    && rm -rf *.apk *.pub \
    && rm -rf /var/cache/apk/*

# 安装JDK
RUN mkdir -p /usr/local/java \
    && mv /home/jre${JAVA_VERSON}/* /usr/local/java \
    && rm -rf /home/jre${JAVA_VERSON}

# 安装字体,语言
RUN cat locale.md | tr -d '\r' | xargs -i /usr/glibc-compat/bin/localedef -i {} -f UTF-8 {}.UTF-8  \
    && rm -rf locale.md \
    && apk add --update ttf-dejavu fontconfig \
    && chmod 777 /usr/share/fonts \
    && fc-cache -fv

# 指定语言
ENV LANG=zh_CN.UTF-8
ENV LANGUAGE=zh_CN.UTF-8

# 检查环境
RUN java -version  \
    && fc-list \
    && /usr/glibc-compat/bin/locale -a

WORKDIR /

CMD ["java","-version"]






