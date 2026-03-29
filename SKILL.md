---
name: knowledge-base-builder
description: 自动从指定目录的文档（.doc, .docx, .pdf）中提取文本，并将它们整合到一个统一、可搜索的知识库文件中。支持Windows、Linux、macOS跨平台使用。用于处理和统一大量源材料，以供研究和分析。
---

# 技能：知识库构建器

## 概述

本技能提供了一个自动化的工作流程，用于将一个包含多种格式文档（`.doc`, `.docx`, `.pdf`）的目录，转换成一个单一、干净、可全文检索的Markdown格式知识库文件。支持Windows、Linux、macOS等多个平台。这对于需要处理大量、格式混杂的原始资料并将其作为后续分析基础的场景非常有用。

## 核心功能

- **多格式支持**：自动识别并处理`.doc`, `.docx`, 和 `.pdf` 文件。
- **跨平台兼容**：支持Windows、Linux、macOS，自动检测平台并使用相应的工具。
- **智能文本提取**：根据平台和可用工具自动选择最优的提取方案。
- **自动整合**：将所有提取的文本整合到一个Markdown文件中，并使用清晰的分隔符标记每个原始文件的来源。

## 依赖要求

### 必需依赖

- **Python 3.7+**
- **python-docx**（用于.docx文件处理）

### 可选依赖（按优先级）

| 依赖 | 用途 | 平台 | 安装方式 |
|------|------|------|---------|
| PyPDF2 | PDF文本提取（推荐，跨平台） | 所有 | `pip install PyPDF2` |
| pdftotext | PDF文本提取（备选） | Linux/Mac | `apt-get install poppler-utils` |
| LibreOffice | .doc文件转换（备选） | Linux/Mac | `apt-get install libreoffice` |

## 安装指南

### Windows

```bash
# 安装Python依赖
pip install python-docx PyPDF2

# 运行脚本
python build_kb.py <source_directory> <output_file_path>
```

### Linux

```bash
# 安装系统依赖（可选，PyPDF2已足够）
sudo apt-get update
sudo apt-get install -y poppler-utils libreoffice

# 安装Python依赖
pip install python-docx PyPDF2

# 运行脚本
python3 build_kb.py <source_directory> <output_file_path>
```

### macOS

```bash
# 使用Homebrew安装系统依赖（可选）
brew install poppler libreoffice

# 安装Python依赖
pip install python-docx PyPDF2

# 运行脚本
python3 build_kb.py <source_directory> <output_file_path>
```

## 使用方法

### 基本用法

```bash
python build_kb.py <source_directory> <output_file_path>
```

**参数说明**：
- `<source_directory>`：包含源文档的目录的绝对路径
- `<output_file_path>`：生成的知识库文件的完整路径

### 示例

```bash
# Windows
python build_kb.py C:\Documents\my_docs C:\output\knowledge_base.md

# Linux/Mac
python3 build_kb.py /home/user/my_docs /home/user/knowledge_base.md
```

## 工作流程

1. **准备资料**：将所有需要整合的源文档放置在一个目录中
2. **安装依赖**：按上述指南安装必需的Python包
3. **执行脚本**：运行`build_kb.py`并指定源目录和输出路径
4. **验证输出**：检查生成的知识库文件

## 特性说明

### 跨平台自适应

脚本会自动检测运行平台，并根据可用的工具选择最优的提取方案：

- **PDF提取优先级**：PyPDF2（跨平台） > pdftotext（Linux/Mac）
- **.doc提取优先级**：python-docx（跨平台） > LibreOffice（Linux/Mac）

### 错误处理

- 如果某个文件提取失败，脚本会记录错误但继续处理其他文件
- 最终输出会包含成功处理的文件数和失败的文件数
- 详细的错误信息会在控制台输出

### 输出格式

生成的知识库文件为Markdown格式，包含：
- 每个源文件的清晰分隔符
- 完整的文本内容
- 处理统计信息

## 注意事项

- **文件兼容性**：对于加密或格式严重损坏的文档，提取过程可能会失败
- **.doc文件**：Windows上对.doc文件的支持有限，建议转换为.docx格式
- **大文件处理**：对于包含大量文件或大体积PDF的目录，处理过程可能需要几分钟时间
- **编码问题**：确保输入文件使用标准编码格式（UTF-8推荐）

## 故障排除

### 问题：ImportError: No module named 'docx'

**解决方案**：
```bash
pip install python-docx
```

### 问题：PDF提取失败

**解决方案**：
```bash
# 安装PyPDF2
pip install PyPDF2

# 或在Linux/Mac上安装pdftotext
sudo apt-get install poppler-utils  # Linux
brew install poppler                # macOS
```

### 问题：.doc文件无法提取（Windows）

**解决方案**：将.doc文件转换为.docx格式后重试

### 问题：输出文件为空或不完整

**解决方案**：
1. 检查源目录路径是否正确
2. 确保有读取权限
3. 查看控制台输出的错误信息
4. 尝试使用绝对路径而非相对路径

## 支持与反馈

- **GitHub仓库**：https://github.com/Blueblood-wq/kacha
- **问题报告**：https://github.com/Blueblood-wq/kacha/issues
- **邮箱**：ericmemo@gmail.com
