---
name: knowledge-base-builder
description: 自动从指定目录的文档（.doc, .docx, .pdf）中提取文本，并将它们整合到一个统一、可搜索的知识库文件中。用于处理和统一大量源材料，以供研究和分析。
---

# 技能：知识库构建器

## 概述

本技能提供了一个自动化的工作流程，用于将一个包含多种格式文档（`.doc`, `.docx`, `.pdf`）的目录，转换成一个单一、干净、可全文检索的Markdown格式知识库文件。这对于需要处理大量、格式混杂的原始资料并将其作为后续分析基础的场景非常有用。

## 核心功能

- **多格式支持**：自动识别并处理`.doc`, `.docx`, 和 `.pdf` 文件。
- **文本提取**：利用`python-docx`, `poppler-utils` (for PDF), 和 `libreoffice` (for .doc) 等工具，最大限度地提取纯文本内容。
- **自动整合**：将所有提取的文本整合到一个Markdown文件中，并使用清晰的分隔符标记每个原始文件的来源。

## 工作流程

使用本技能构建知识库的流程如下：

1.  **准备资料**：将所有需要整合的源文档放置在一个独立的目录中（例如 `/home/ubuntu/source_materials/`）。
2.  **安装依赖**：确保所有必需的系统依赖都已安装。如果未安装，执行以下命令：
    ```bash
    sudo apt-get update -qq && sudo apt-get install -y poppler-utils libreoffice -qq && sudo pip3 install python-docx
    ```
3.  **执行脚本**：调用核心脚本 `build_kb.py`，并提供源目录和期望的输出文件名。
4.  **验证输出**：检查生成的知识库文件，确保内容完整且格式正确。

## 核心脚本：`build_kb.py`

本技能的核心是一个名为 `build_kb.py` 的Python脚本，它负责整个构建过程。

- **位置**: `/home/ubuntu/skills/knowledge-base-builder/scripts/build_kb.py`

### 使用方法

该脚本通过命令行接收两个参数：

```bash
/home/ubuntu/skills/knowledge-base-builder/scripts/build_kb.py <source_directory> <output_file_path>
```

- `<source_directory>`: 包含源文档的目录的绝对路径。
- `<output_file_path>`: 生成的知识库文件的完整路径（例如 `/home/ubuntu/my_knowledge_base.md`）。

### 示例

假设您的所有文档都存放在 `/home/ubuntu/my_docs` 目录下，您希望生成一个名为 `project_kb.md` 的知识库文件，可以运行以下命令：

```bash
/home/ubuntu/skills/knowledge-base-builder/scripts/build_kb.py /home/ubuntu/my_docs /home/ubuntu/project_kb.md
```

脚本将自动处理目录中的所有支持文件，并在完成后于 `/home/ubuntu/` 目录下生成 `project_kb.md`。

## 注意事项

- **文件兼容性**：对于加密或格式损坏的文档，提取过程可能会失败。建议在开始前确保文件可正常打开。
- **环境依赖**：本技能依赖于多个系统级软件包。在干净的环境中首次使用时，必须先运行依赖安装命令。
- **处理时间**：对于包含大量文件或大体积PDF的目录，处理过程可能需要几分钟时间。请耐心等待脚本执行完毕。
