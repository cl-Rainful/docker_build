# 使用一个基础的 Java + Maven 环境
FROM openjdk:11-jdk as build

# 设置环境变量
ENV DORIS_VERSION=2.1.7
ENV DORIS_HOME=/opt/apache-doris

# 安装必需的构建工具和依赖
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    git \
    maven \
    cmake \
    wget \
    lsb-release \
    python3 \
    python3-pip && \
    rm -rf /var/lib/apt/lists/*

# 下载并解压 Doris 源代码
RUN mkdir -p $DORIS_HOME && \
    cd /opt && \
    wget https://dist.apache.org/repos/dist/release/doris/2.1/2.1.7/apache-doris-2.1.7-rc03-src.tar.gz && \
    tar -xzvf apache-doris-2.1.7-rc03-src.tar.gz -C $DORIS_HOME --strip-components=1 && \
    rm apache-doris-2.1.7-rc03-src.tar.gz

# 编译 Doris
WORKDIR $DORIS_HOME
RUN sh build.sh

# --- 创建运行时镜像 ---
FROM ubuntu:20.04

# 设置环境变量
ENV DORIS_HOME=/opt/apache-doris
ENV PATH=$DORIS_HOME/bin:$PATH

# 安装必要的依赖
RUN apt-get update && \
    apt-get install -y \
    openjdk-11-jdk \
    python3 && \
    rm -rf /var/lib/apt/lists/*

# 将编译好的 Doris 二进制文件复制到运行时镜像中
COPY --from=build /opt/apache-doris/output/ /opt/apache-doris/

# 暴露 Doris 服务的端口
EXPOSE 8030 8031 8040 8041

# 启动 Doris
CMD ["sh", "-c", "$DORIS_HOME/bin/start_be.sh && $DORIS_HOME/bin/start_fe.sh && tail -f /dev/null"]
