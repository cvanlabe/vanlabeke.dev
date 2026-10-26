---
name: exshell
description: Read and review CSV, TSV and Excel (.xlsx/.xlsm) files from the shell with the exshell CLI. Use when the user asks to look at, inspect, summarize, check or review a spreadsheet, workbook or delimited data file, or when a task needs the contents of a .csv/.tsv/.xlsx file.
---

# Reading tabular files with exshell

exshell is a read-only command-line viewer for CSV, TSV and Excel files. Piped, it prints plain, aligned, untruncated text: a header line, a dashed rule line, then one line per row.

Full reference: https://vanlabeke.dev/exshell/llms.txt

## Steps

1. Check that it is installed: `command -v exshell`. If it is missing, ask the user before installing (`brew install vanlabeke/tap/exshell` after `brew tap vanlabeke/tap && brew trust --formula vanlabeke/tap/exshell`, or see https://vanlabeke.dev/exshell/#install).
2. For a workbook, list the sheets first: `exshell --list-sheets file.xlsx`.
3. Read with `--print` so the interactive viewer never opens:
   - `exshell --print file.csv`
   - `exshell --print --sheet "Sheet name" file.xlsx`
4. For big files, size up first and read in slices instead of all at once:
   - `exshell --print file.csv | wc -l` (rows = lines − 2)
   - `exshell --print file.csv | head -40`
   - `exshell --print file.csv | grep -i "term"`
5. If columns look merged or split wrongly, set the delimiter: `--delim ';'`, `--delim $'\t'`. If text is garbled, try `--encoding latin1` or `--encoding utf16`. If row 1 is data rather than headers, add `--no-header`.

## Notes

- Values are what Excel displays: formatted dates, currency and percentages, and cached formula results.
- Do not pass `--max-col-width`; it truncates cells.
- Input must be a file path; stdin is not supported.
- Exit code 1 means the file could not be read (missing, password-protected, or a legacy `.xls`). Exit code 2 means a bad flag or sheet. The message is on stderr.
- exshell cannot sort, filter or edit. Use `sort`, `grep`, `awk` or a data library on its output for that.
