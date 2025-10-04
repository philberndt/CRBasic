# CRBasic documentation index

This repository holds the documentation for several Campbell Scientific CRBasic datalogger series and provides LLM-friendly entry points for automated consumption.

Each series folder under `docs/` includes human-readable documentation and a curated `llms.txt` file (plus companion enumerator files where sections are large). These `llms.txt` files are intended as concise, machine-friendly starting points for indexing or LLM ingestion.

Per-series LLM entry points

- CRbasic1x
  - `docs/CRbasic1x/llms.txt`
  - `docs/CRbasic1x/instructions-llms.txt` (complete)
  - `docs/CRbasic1x/parameters-llms.txt` (complete)
- CRbasic300
  - `docs/CRbasic300/llms.txt`
  - `docs/CRbasic300/instructions-llms.txt` (complete)
  - `docs/CRbasic300/parameters-llms.txt` (complete)
- CRbasic350
  - `docs/CRbasic350/llms.txt`
  - `docs/CRbasic350/instructions-llms.txt` (complete)
  - `docs/CRbasic350/parameters-llms.txt` (complete)

Why we use `llms.txt`

- `llms.txt` is a simple Markdown convention for providing concise documentation entry points that are easy for automated tools and LLMs to consume.
- When a subtree contains many files (for example hundreds of instruction or parameter pages), we keep `llms.txt` succinct and place exhaustive, alphabetized lists in companion files `instructions-llms.txt` and `parameters-llms.txt` to avoid overwhelming ingestion pipelines.

**Disclaimer**: This documentation was converted from an extracted .chm file and batch converted to Markdown using markdownify. I have tried to ensure that the content is accurate but there might be some errors or omissions. Please review the content carefully before using it for any critical purposes. The content in this repository is provided as-is and is not guaranteed to be error-free or complete. While every effort has been made to ensure accuracy, users are advised to verify the information before relying on it for any critical purposes.
