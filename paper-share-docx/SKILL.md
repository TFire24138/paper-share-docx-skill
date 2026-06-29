---
name: paper-share-docx
description: Read an academic paper and create a Chinese paper-sharing report as a polished DOCX, especially when the user provides an arXiv/PDF/HTML paper link plus a prior report to imitate. Use for tasks like “阅读这篇论文并写成汇报报告”, “参考我之前的论文分享稿生成 docx”, “生成可导入飞书云文档的论文分享文档”, including optional cleanup of intermediate Markdown/images after the DOCX is verified.
---

# Paper Share DOCX

## Goal

Produce a self-contained `.docx` paper-sharing report in Chinese, suitable for importing into Feishu/Lark Docs. Preserve the user's prior presentation style when a reference report is provided, while grounding technical content in the target paper.

Use the `documents` skill for DOCX creation/verification whenever available. If a paper URL or current paper metadata is involved, browse or otherwise fetch the source; do not rely on memory.

## Workflow

### 1. Ground the Inputs

Collect and verify:

- Target paper source: arXiv HTML/PDF/e-print, local PDF, conference page, or repository.
- Reference report: `.md`, `.docx`, `.pptx`, `.zip`, or folder from a prior paper share.
- Desired output: default to `<PaperShortName>.docx` on the user's working directory.
- Cleanup request: delete intermediate Markdown/images only when explicitly requested and only after DOCX validation.

For arXiv papers:

- Open the HTML page for readable structure and citations.
- Download the PDF or e-print source when figures, tables, formulas, or appendices matter.
- Record title, authors, institutions, version/date, code link, and main claims.

For reference reports:

- Extract the structure and tone, not the subject matter.
- Identify the user's usual sequence, e.g. `任务定义 -> Introduction -> Related Work -> Method -> Experiment -> 消融 -> 局限/个人理解 -> 讲稿提纲`.
- Note density, language style, how figures are introduced, and whether the report is more like a talk script or formal note.

### 2. Read the Paper Like a Presenter

Build a concise source map before drafting:

- **Problem**: What task/field is the paper addressing?
- **Motivation**: What empirical failure or gap motivates the method?
- **Method**: What is the key mechanism, preferably in 1-3 memorable phrases?
- **Experiments**: What questions do the experiments answer?
- **Numbers**: Which exact results are worth speaking aloud?
- **Limitations**: What assumptions, cost, domain boundaries, or missing validations matter?

Prefer a talk-friendly explanation over paragraph-by-paragraph translation. Preserve formulas only when they clarify the method. For dense ML/RL papers, usually keep:

- One or two symbolic summaries of the objective.
- The direction of any weighting/scoring rule.
- The final “how to explain this to classmates” interpretation.

### 3. Create Intermediate Assets

When the paper has useful figures:

- Extract figures from the paper source or render PDF figures to PNG.
- Use readable filenames such as `figure_1_motivation.png`, `figure_2_framework.png`, `table_main_results_summary.png`.
- If original tables are too wide for a report, create a compact summary table image or native DOCX table with only the key rows/metrics.

For Feishu import, prefer embedded DOCX images over Markdown-local image links. The final `.docx` must not depend on a sibling image directory.

### 4. Draft the Report

Default Chinese structure:

1. Title and paper metadata
2. 汇报节奏 or one-line speaking plan
3. 任务定义 / Background
4. Introduction / Core problem
5. Related Work, grouped by method families
6. Preliminary analysis or motivating observations
7. Method, explained as an executable story
8. Experiments, organized by the questions they answer
9. Ablation / extended experiments / cost
10. Limitations and personal interpretation
11. Talk outline and discussion questions
12. References

Writing style:

- Use clear Chinese explanations with light oral phrasing.
- Prefer “这篇论文真正想解决的问题是...” and “这部分实验主要回答...” style transitions.
- Highlight the narrative spine early, then repeat it in method and conclusion.
- Avoid untranslated technical walls; define terms like `rollout`, `verifier`, `Pass@k`, `PPL`, or `KL` when the audience may not know them.
- Keep exact numeric results close to their interpretation.

### 5. Build the DOCX

Use a restrained technical-report layout:

- Preset: use `compact_reference_guide` unless the user asks for a formal memo or Google Docs-native style.
- Page: Letter portrait, 1 inch margins.
- Typography: readable Chinese-compatible fonts; consistent heading hierarchy.
- Figures: centered, width constrained to page, with enough spacing.
- Tables: use native DOCX tables only for genuinely tabular summaries; otherwise use bullets/prose.
- Code/formula blocks: use shaded monospace paragraphs, not screenshots, unless the original formula visual is important.

If converting from Markdown:

- Ensure images are embedded with `add_picture` or equivalent.
- Convert links to real hyperlinks where possible.
- Avoid page-break artifacts that Quick Look or Feishu may render as small squares.
- Make the DOCX self-contained before deleting any source assets.

### 6. Verify Before Delivery

Required checks:

- Open/parse the DOCX with `python-docx` or equivalent.
- Confirm paragraph count is nonzero, expected tables exist, and every intended image is embedded under `word/media`.
- Confirm key claims and numbers appear in the final document XML/text.
- Render with the `documents` skill renderer when LibreOffice and dependencies are available.
- If full render is unavailable, use fallback checks such as macOS Quick Look thumbnail, file metadata, embedded media count, and structural XML checks. Disclose the fallback in the final response.

Before cleanup:

- Verify the final DOCX exists and is not tiny.
- Verify embedded image count matches the expected asset count.
- Verify the DOCX opens or previews.
- Only then remove intermediate `.md` files and image directories, and only when the user explicitly requested deletion.

### 7. Final Response

Return a concise completion note with:

- Link to the final `.docx`.
- What was deleted, if cleanup was requested.
- Verification performed.
- Any verification limitation, e.g. “LibreOffice render QA was unavailable, so I used structural checks and Quick Look preview.”

Do not return intermediate PNG/PDF paths unless the user asks for them.

## Quality Bar

The report should let the user present the paper without rereading the original. It should contain:

- A clear one-sentence thesis.
- A talk-friendly explanation of the method.
- The key figures or compact replacements.
- Exact results for the claims made.
- A balanced limitations/personal-understanding section.
- A short talk outline that can be used as speaker notes.
