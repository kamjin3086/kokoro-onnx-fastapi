# 使用官方 Python 运行时作为父镜像
FROM python:3.12-slim

# 更换为清华源 + 安装 uv（推荐做法，一次配置后续都受益）
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn && \
    pip install --no-cache-dir uv

# 设置工作目录
WORKDIR /app

# 安装项目依赖（此时已经使用清华源）
COPY requirements.txt .
RUN uv pip install --no-cache-dir -r requirements.txt --system

# 复制应用代码
COPY main.py .
COPY download_deps.py .
COPY cache.py .
# COPY . .          # 如需复制全部文件可打开此行

# 创建持久化目录（模型存放目录 + 生成音频目录）
RUN mkdir -p /app/models && mkdir -p /app/generated_audio

# 暴露端口
EXPOSE 8211

# 启动命令（保持原样，确保 download_deps.py 会在启动时执行）
CMD ["uv", "run", "python", "main.py"]