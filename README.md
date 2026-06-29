# Paper Share DOCX Skill

<p align="center">
  <strong>Turn academic papers into polished Chinese DOCX presentation reports for Feishu/Lark Docs.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-yellow.svg"></a>
  <img alt="Codex Skill" src="https://img.shields.io/badge/Codex-Skill-111827">
  <img alt="Output" src="https://img.shields.io/badge/output-DOCX-blue">
  <img alt="Language" src="https://img.shields.io/badge/report%20language-Chinese-red">
</p>

`paper-share-docx` is a Codex skill for one very specific, very common research workflow:

> Read a paper, learn the style of a previous paper-sharing report, and produce a self-contained `.docx` that can be uploaded to Feishu/Lark Docs.

It is designed for students, researchers, and engineers who regularly need to present papers in lab meetings, reading groups, or internal sharing sessions.

## Why This Skill Exists

Academic papers are dense. Paper-sharing documents need a different shape:

- They must explain the paper like a talk, not translate it paragraph by paragraph.
- They need a clear story: problem, motivation, method, experiments, limitations.
- They should preserve important figures and exact results.
- They should be easy to upload into Feishu/Lark Docs without broken local image links.

This skill captures that workflow as a repeatable Codex procedure.

## What It Does

- Reads papers from arXiv HTML/PDF/e-print, local PDFs, conference pages, or repositories.
- Extracts the paper's title, authors, institutions, version, code link, method, experiments, and limitations.
- Learns structure and tone from a prior report, such as a Markdown/DOCX/PPTX/ZIP paper-sharing file.
- Writes a Chinese presentation-style report with speaker-friendly explanations.
- Builds a self-contained `.docx` with embedded figures.
- Verifies the generated DOCX before delivery.
- Optionally deletes intermediate Markdown and image assets after the DOCX is confirmed.

## Output Style

The default report structure is:

```text
Title and paper metadata
汇报节奏 / speaking plan
任务定义 / Background
Introduction / core problem
Related Work
Preliminary analysis or motivation
Method
Experiments
Ablation / extended experiments / cost
Limitations and personal interpretation
Talk outline and discussion questions
References
```

The writing style is intentionally talk-friendly:

- clear Chinese explanations,
- light oral transitions,
- exact numbers near the claims they support,
- formulas only when they help the audience understand the method,
- figures embedded in the final DOCX.

## Installation

Copy the skill folder into your Codex skills directory:

```bash
cp -R paper-share-docx ~/.codex/skills/
```

Then start a new Codex session so the skill can be discovered.

Your local skill path should look like this:

```text
~/.codex/skills/
└── paper-share-docx/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## Usage

Use it by name in your prompt:

```text
使用 paper-share-docx，阅读这篇论文：
https://arxiv.org/html/2604.10688

参考 /Users/me/Desktop/ReconVLA.zip 里我之前的汇报风格，
生成一份可导入飞书云文档的中文 DOCX 论文分享稿。
```

You can also ask it to clean up intermediate files:

```text
生成 DOCX 后，把中间的 Markdown 和图片文件夹删掉，只保留最终 docx。
```

## Recommended Inputs

For best results, provide:

| Input | Example | Why it helps |
|---|---|---|
| Target paper | arXiv HTML/PDF link or local PDF | Gives the skill a source of truth |
| Prior report | `.md`, `.docx`, `.pptx`, `.zip`, or folder | Lets the skill imitate your reporting style |
| Audience | classmates, lab group, engineers, etc. | Adjusts depth and vocabulary |
| Duration | 10 min, 15-20 min, 30 min | Controls detail level |
| Cleanup preference | keep or delete intermediates | Prevents accidental loss of useful drafts |

## Example Prompts

```text
使用 paper-share-docx，读这篇 arXiv 论文，参考我之前的 ReconVLA 汇报，
生成一份 15-20 分钟中文论文分享 DOCX。
```

```text
用 paper-share-docx 把这个 PDF 写成组会分享稿。
听众懂基本机器学习，但不熟悉强化学习。
请保留关键图表和核心公式，最后给一页讲稿提纲。
```

```text
阅读这篇论文和我的旧 PPT，输出飞书可导入的 docx。
生成后删除中间 md 和图片，只保留 docx。
```

## Repository Layout

```text
paper-share-docx-skill/
├── README.md
├── LICENSE
├── .gitignore
└── paper-share-docx/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## How It Works

The skill guides Codex through seven stages:

1. **Ground the inputs**  
   Verify the paper source, reference report, output path, and cleanup request.

2. **Read the paper like a presenter**  
   Build a source map: problem, motivation, method, experiments, numbers, limitations.

3. **Create intermediate assets**  
   Extract or render useful figures and build compact result summaries when tables are too wide.

4. **Draft the report**  
   Write a Chinese talk-style document, preserving the user's prior report rhythm.

5. **Build the DOCX**  
   Generate a self-contained Word document with embedded images.

6. **Verify before delivery**  
   Check text, embedded media, key claims, and render/preview when available.

7. **Deliver and clean up**  
   Return the final `.docx`; delete intermediate files only when explicitly requested.

## Design Principles

- **Source-grounded**: fetch and inspect the paper instead of relying on memory.
- **Presentation-first**: explain what the audience needs to understand, not every paragraph.
- **Style-aware**: imitate the user's previous report structure and density.
- **Feishu-friendly**: prefer embedded DOCX images over Markdown-local image paths.
- **Safe cleanup**: never delete intermediate files until the final DOCX is verified.

## Compatibility

This repository contains a Codex skill. It assumes an environment where Codex can discover skills from:

```text
~/.codex/skills/
```

The skill may also call other available capabilities when present, especially document-generation and rendering tools for `.docx` quality checks.

## Contributing

Contributions are welcome. Good improvements include:

- better DOCX layout guidance,
- more robust Feishu/Lark import notes,
- examples for different paper types,
- stronger validation checklists,
- multilingual report variants.

Before opening a pull request:

1. Keep `SKILL.md` concise and procedural.
2. Avoid adding large generated artifacts.
3. Make sure the skill still validates in your local Codex skill environment.

## License

MIT License. See [LICENSE](LICENSE) for details.

## Acknowledgements

This README follows common open-source documentation patterns: a clear one-line value proposition, quick installation, concrete usage examples, repository layout, contribution notes, and a visible license.
