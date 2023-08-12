FROM alpine:3.15

MAINTAINER 852203465@qq.com

ENV GCC_LIBS_VERSION=12.2.1-4
ENV ZLIB_VERSION=1_1.2.13-3
ENV GLIBC_VERSION=2.33-r0
ENV JAVA_VERSON=17.0.8
ENV JAVA_HOME=/usr/local/java
ENV CLASSPATH=.:$JAVA_HOME/lib
ENV PATH=$PATH:$JAVA_HOME/bin

# 设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

WORKDIR /home

ADD ./jdk-${JAVA_VERSON}_linux-x64_bin.tar.gz /home
COPY ./gcc-libs-${GCC_LIBS_VERSION}-x86_64.pkg.tar.zst /home
COPY ./zlib-${ZLIB_VERSION}-x86_64.pkg.tar.zst /home
ADD ./Fonts.tar.gz /usr/share/fonts/
COPY ./locale.md /home/locale.md
COPY ./glibc* /home
COPY ./sgerrand.rsa.pub /etc/apk/keys/sgerrand.rsa.pub

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories  \
    && apk update  \
    && apk --no-cache add ca-certificates tini binutils zstd tar \
    && apk add glibc-bin-${GLIBC_VERSION}.apk glibc-i18n-${GLIBC_VERSION}.apk glibc-${GLIBC_VERSION}.apk  \
    && rm -rf *.apk \
    && rm -rf /var/cache/apk/*

RUN mkdir -p /tmp/gcc-libs  \
    && tar -I zstd -xvf gcc-libs-${GCC_LIBS_VERSION}-x86_64.pkg.tar.zst -C /tmp/gcc-libs  \
    && mv /tmp/gcc-libs/usr/lib/libgcc* /tmp/gcc-libs/usr/lib/libstdc++* /usr/glibc-compat/lib \
    && strip /usr/glibc-compat/lib/libgcc_s.so.* /usr/glibc-compat/lib/libstdc++.so*  \
    && mkdir -p /tmp/zlib  \
    && tar -I zstd -xvf zlib-${ZLIB_VERSION}-x86_64.pkg.tar.zst -C /tmp/zlib  \
    && mv /tmp/zlib/usr/lib/libz.so* /usr/glibc-compat/lib  \
    && apk del binutils zstd \
    && rm -rf /tmp/gcc-libs /tmp/zlib

# 安装JDK
RUN mkdir -p /usr/local/java \
    && mv /home/jdk-${JAVA_VERSON}/* /usr/local/java \
    && rm -rf /home/jdk-${JAVA_VERSON}

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






