# 蓝血技能库

华为及任正非相关内容的Manus技能集合。

## 项目简介

本仓库包含为Manus AI平台开发的一系列高质量技能（Skills），专注于华为管理思想、任正非讲话、企业文化等领域的知识处理和内容创作。

## 包含的技能

### 1. 知识库构建器 (Knowledge Base Builder)

**功能**：自动从多种格式的文档（.doc, .docx, .pdf）中提取文本，并整合成一个统一、可搜索的知识库文件。

**适用场景**：
- 处理大量混合格式的源材料
- 为研究和分析构建统一的知识基础
- 快速整合企业内部文档

**使用方法**：

```bash
/home/ubuntu/skills/knowledge-base-builder/scripts/build_kb.py <source_directory> <output_file_path>
```

**参数说明**：
- `<source_directory>`：包含源文档的目录的绝对路径
- `<output_file_path>`：生成的知识库文件的完整路径

**示例**：

```bash
/home/ubuntu/skills/knowledge-base-builder/scripts/build_kb.py /home/ubuntu/my_docs /home/ubuntu/project_kb.md
```

## 技能安装

### 方式一：从GitHub克隆

```bash
git clone https://github.com/Blueblood-wq/蓝血技能.git
cd 蓝血技能
```

### 方式二：在Manus中添加技能

将技能目录添加到您的Manus技能库中：

1. 将 `knowledge-base-builder` 目录复制到 `/home/ubuntu/skills/` 目录下
2. 在Manus中使用该技能

## 技能结构

```
knowledge-base-builder/
├── SKILL.md                 # 技能说明文档（包含YAML元数据）
├── scripts/
│   └── build_kb.py         # 核心执行脚本
├── references/             # 参考文档目录（预留）
└── templates/              # 模板文件目录（预留）
```

## 依赖要求

### 系统依赖

- `poppler-utils`：用于PDF文本提取
- `libreoffice`：用于.doc文件转换
- Python 3.7+

### Python依赖

- `python-docx`：用于.docx文件处理

### 安装依赖

```bash
sudo apt-get update && sudo apt-get install -y poppler-utils libreoffice
sudo pip3 install python-docx
```

## 使用示例

### 场景：整合华为管理资料

假设您有一个包含华为相关文档的目录 `/home/ubuntu/huawei_docs/`，其中包含：
- 任正非内部讲话（.doc格式）
- 高管文章（.docx格式）
- 研究报告（.pdf格式）

执行以下命令将这些资料整合成一个知识库：

```bash
/home/ubuntu/skills/knowledge-base-builder/scripts/build_kb.py /home/ubuntu/huawei_docs /home/ubuntu/huawei_knowledge_base.md
```

完成后，您将获得一个名为 `huawei_knowledge_base.md` 的文件，其中包含所有提取的文本内容，并用清晰的分隔符标记每个原始文件的来源。

## 注意事项

- **文件兼容性**：对于加密或格式损坏的文档，提取过程可能会失败。建议在开始前确保文件可正常打开。
- **处理时间**：对于包含大量文件或大体积PDF的目录，处理过程可能需要几分钟时间。
- **输出格式**：生成的知识库文件为Markdown格式，可直接在任何Markdown编辑器中打开和编辑。

## 技能开发工作流

本技能是使用Manus的 `skill-creator` 技能开发的。如需修改或扩展功能，请参考 `skill-creator` 的文档。

## 许可证

本仓库中的所有技能遵循Manus平台的标准许可协议。

## 贡献指南

欢迎提交Issue和Pull Request来改进这些技能。

## 联系方式

- GitHub用户名：Blueblood-wq
- 邮箱：ericmemo@gmail.com

## 更新日志

### v1.0 (2026-03-28)

- 初始版本发布
- 包含知识库构建器技能
- 支持.doc, .docx, .pdf文件格式
