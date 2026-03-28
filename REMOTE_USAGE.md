# 远程调用指南

本文档说明如何从GitHub仓库远程调用"知识库构建器"技能。

## 方案一：直接从GitHub克隆并使用

### 步骤1：克隆仓库

```bash
git clone git@github.com:Blueblood-wq/-.git
cd -
```

### 步骤2：安装依赖

```bash
sudo apt-get update && sudo apt-get install -y poppler-utils libreoffice
sudo pip3 install python-docx
```

### 步骤3：运行脚本

```bash
./knowledge-base-builder/scripts/build_kb.py <source_directory> <output_file_path>
```

**示例**：

```bash
./knowledge-base-builder/scripts/build_kb.py /home/ubuntu/my_docs /home/ubuntu/my_kb.md
```

## 方案二：在Manus中集成远程技能

### 步骤1：在Manus中添加技能

在Manus平台中，您可以直接从GitHub仓库URL添加技能。使用以下URL：

```
https://github.com/Blueblood-wq/-.git/tree/main/knowledge-base-builder
```

或者使用SSH URL：

```
git@github.com:Blueblood-wq/-.git/tree/main/knowledge-base-builder
```

### 步骤2：使用技能

添加后，Manus会自动识别 `SKILL.md` 文件，您可以在需要时调用该技能。

## 方案三：使用原始文件URL

GitHub提供了原始文件的直接访问URL。您可以通过以下方式访问技能文件：

### 获取SKILL.md

```
https://raw.githubusercontent.com/Blueblood-wq/-/main/knowledge-base-builder/SKILL.md
```

### 获取脚本文件

```
https://raw.githubusercontent.com/Blueblood-wq/-/main/knowledge-base-builder/scripts/build_kb.py
```

## 方案四：使用curl下载并执行

如果您想在任何地方快速使用该脚本，可以使用curl直接下载并执行：

```bash
# 下载脚本
curl -o build_kb.py https://raw.githubusercontent.com/Blueblood-wq/-/main/knowledge-base-builder/scripts/build_kb.py

# 赋予执行权限
chmod +x build_kb.py

# 运行脚本
python3 build_kb.py <source_directory> <output_file_path>
```

## 方案五：Docker容器化部署

如果您需要在Docker中运行，可以创建一个Dockerfile：

```dockerfile
FROM python:3.11-slim

# 安装系统依赖
RUN apt-get update && apt-get install -y \
    poppler-utils \
    libreoffice \
    git \
    && rm -rf /var/lib/apt/lists/*

# 安装Python依赖
RUN pip install python-docx

# 克隆仓库
RUN git clone git@github.com:Blueblood-wq/-.git /app
WORKDIR /app

# 创建输出目录
RUN mkdir -p /data/input /data/output

# 设置入口点
ENTRYPOINT ["python", "knowledge-base-builder/scripts/build_kb.py"]
CMD ["/data/input", "/data/output/knowledge_base.md"]
```

使用方法：

```bash
# 构建镜像
docker build -t knowledge-base-builder .

# 运行容器
docker run -v /path/to/your/docs:/data/input \
           -v /path/to/output:/data/output \
           knowledge-base-builder
```

## 方案六：GitHub Actions自动化

您可以创建一个GitHub Actions工作流，自动处理上传到特定目录的文档。

创建 `.github/workflows/build-kb.yml`：

```yaml
name: Build Knowledge Base

on:
  push:
    paths:
      - 'documents/**'
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install dependencies
      run: |
        sudo apt-get update
        sudo apt-get install -y poppler-utils libreoffice
        pip install python-docx
    
    - name: Build knowledge base
      run: |
        python knowledge-base-builder/scripts/build_kb.py \
          ./documents \
          ./output/knowledge_base.md
    
    - name: Commit and push
      run: |
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git add output/knowledge_base.md
        git commit -m "Auto-generated knowledge base"
        git push
```

## 最佳实践

### 1. 使用SSH密钥而非HTTPS

对于频繁的远程操作，建议使用SSH密钥而非Personal Access Token，因为SSH密钥更安全且不容易过期。

### 2. 定期更新仓库

如果您在本地克隆了仓库，请定期运行以下命令以获取最新版本：

```bash
git pull origin main
```

### 3. 版本控制

如果您对脚本进行了修改，建议创建一个新的分支：

```bash
git checkout -b my-improvements
# 进行修改...
git commit -am "Add my improvements"
git push origin my-improvements
```

然后向原始仓库提交Pull Request。

### 4. 错误处理

在使用脚本时，建议检查返回值：

```bash
./knowledge-base-builder/scripts/build_kb.py /source /output
if [ $? -eq 0 ]; then
    echo "Knowledge base built successfully"
else
    echo "Error building knowledge base"
fi
```

### 5. 日志记录

为了调试和监控，建议将输出重定向到日志文件：

```bash
./knowledge-base-builder/scripts/build_kb.py /source /output > kb_build.log 2>&1
```

## 故障排除

### 问题1：SSH连接失败

**症状**：`Permission denied (publickey)`

**解决方案**：
1. 确保您的SSH公钥已添加到GitHub账户
2. 测试连接：`ssh -T git@github.com`
3. 如果仍然失败，重新生成SSH密钥

### 问题2：脚本权限不足

**症状**：`Permission denied`

**解决方案**：
```bash
chmod +x knowledge-base-builder/scripts/build_kb.py
```

### 问题3：依赖安装失败

**症状**：`libreoffice: command not found`

**解决方案**：
```bash
sudo apt-get update
sudo apt-get install -y libreoffice poppler-utils
```

### 问题4：PDF提取失败

**症状**：PDF文件未被正确处理

**解决方案**：
1. 确保PDF文件未被加密
2. 尝试使用其他PDF查看器打开文件以验证其完整性
3. 查看脚本的错误消息以获取更多信息

## 支持与反馈

如有问题或建议，请在GitHub仓库中提交Issue或Pull Request。

- **仓库地址**：https://github.com/Blueblood-wq/-
- **问题报告**：https://github.com/Blueblood-wq/-/issues
- **邮箱**：ericmemo@gmail.com
