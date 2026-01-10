# 使用官方 Python 运行时作为父镜像
FROM python:3.12-slim

# 更换为清华源 + 安装 uv
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    pip install --no-cache-dir uv

# 设置工作目录
WORKDIR /app

# 安装依赖（使用已配置好的清华源）
COPY requirements.txt .
RUN uv pip install --no-cache-dir -r requirements.txt --system

# 复制应用代码
COPY main.py .
COPY cache.py .
COPY download_deps.py .
# COPY . .   # 如果需要复制全部文件可打开此行

# 创建持久化目录
RUN mkdir -p /app/models && mkdir -p /app/generated_audio

# 暴露端口
EXPOSE 8210

# 推荐使用这种方式启动（确保 download_deps.py 逻辑被执行）
CMD ["uv", "run", "python", "main.py"]
