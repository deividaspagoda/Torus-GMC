---
name: sample-data
description: Add, change or validate sample data files (sample.json, sample.xml or a new sample.<format>) in the Torus-GMC repository. Use when the user asks to create a sample file in a new format, edit sample data, or check that the sample files are valid.
---

# Sample data files

The repository holds sample data only. Every sample file describes the same record, so the formats stay comparable.

## Shared record

Use `sample.json` as the source of truth. A sample file in any format carries these fields:

- `name`, `description`, `version`, `createdAt`
- `author.username`
- `settings`: `enabled`, `maxItems`, `tags` (the last tag names the file's format, e.g. `json`, `xml`)
- `items`: each with `id`, `title`, `active`

When you change a value in one sample file, change it in every other sample file too, unless the user asks for only one.

## Adding a sample file

1. Name it `sample.<extension>` in the repository root.
2. Write the shared record in that format, using the format's normal idioms (for XML, `id` and `active` are attributes on `<item>`).
3. Indent with 2 spaces.
4. Validate it (see below).
5. Add a row for it to the **Contents** table in `README.md`.

## Validating

Validate every sample file you create or change before reporting it done.

PowerShell:

```powershell
Get-Content -Raw sample.json | ConvertFrom-Json | Out-Null   # JSON
[xml](Get-Content -Raw sample.xml) | Out-Null                # XML
```

Python, if available:

```sh
python -c "import json,sys; json.load(open(sys.argv[1]))" sample.json
python -c "import xml.dom.minidom,sys; xml.dom.minidom.parse(sys.argv[1])" sample.xml
```

For any other format, use that format's standard parser. A command that finishes with no error means the file is valid.

## Don't

- Put real personal data, secrets or credentials in sample files.
- Commit unless the user asks.
