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

Quick start

- For automatic ingestion, point your agent or pipeline at the `llms.txt` for the desired series (for example `https://philberndt.github.io/CRBasic-Docs/CRbasic350/llms.txt`).
- For manual browsing, open the `llms.txt` in the series folder. Use the companion enumerator files for complete lists.

Link validation (PowerShell)

Run this PowerShell snippet from the repository root to check relative links referenced in `*-llms.txt` files. It reports missing targets (case-sensitive on some filesystems):

```powershell
$base = "docs"
Get-ChildItem -Recurse -Filter "*-llms.txt" $base | ForEach-Object {
  $file = $_.FullName
  Write-Host "Checking: $file"
  (Get-Content $file) | ForEach-Object {
    if ($_ -match '\(\.\/([^\)]+)\)') {
      $rel = $matches[1]
      $target = Join-Path -Path (Split-Path $file) -ChildPath $rel
      if (-not (Test-Path $target)) { Write-Host " MISSING: $rel referenced from $file" -ForegroundColor Red }
    }
  }
}
```

Next steps you might want

- Add one-line summaries to companion enumerator files (extract first paragraph from each doc) — I can generate these if you want.
- Add a simple GitHub Action to validate links on pull requests — I can provide a minimal workflow.
- Modify which Info pages are highlighted in each `llms.txt` — I can tune these to your preference.

If you'd like any of the above (or additional examples for ingestion code), tell me which and I'll implement it.

- For manual browsing: open the `llms.txt` in the series folder, or open the companion `instructions-llms.txt` / `parameters-llms.txt` files for the full lists.
