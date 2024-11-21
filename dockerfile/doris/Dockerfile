# --- 创建运行时镜像 ---
FROM ubuntu:20.04

# 设置环境变量
ENV DORIS_HOME=/opt/apache-doris
ENV PATH=$DORIS_HOME/bin:$PATH

# 安装必要的依赖
RUN apt-get update && \
    apt-get install -y \
    openjdk-8-jdk \
    python3 && \
    rm -rf /var/lib/apt/lists/*

# 将编译好的 Doris 二进制文件复制到运行时镜像中
COPY --from=build /opt/apache-doris/output/ /opt/apache-doris/

# 暴露 Doris 服务的端口
EXPOSE 8030 8031 8040 8041

# 启动 Doris
CMD ["sh", "-c", "$DORIS_HOME/bin/start_be.sh && $DORIS_HOME/bin/start_fe.sh && tail -f /dev/null"]
