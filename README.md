# Enrich TEI files with lemma and POS tags  

Python script to enrich TEI XML files with linguistic annotations (lemma and POS tags) from Excel spreadsheets.

## Overview

This tool reads token annotations from an Excel file and adds them as attributes to `<tok>` elements in TEI XML documents. It matches tokens by their `id` attribute and enriches them with `lemma` and `tag` (POS) information.

## Requirements

- Python 3.6+
- openpyxl

Install dependencies:
```bash
pip install openpyxl
```

## Usage

### Basic usage (with defaults)

```bash
python3 scripts/add_lemmas.py
```

This uses the default files:
- Tagged file: `data/HdE_A.tagged.xlsx`
- Input XML: `data/HdE_A.xml`
- Output XML: `HdE_A.with_lemmas.xml`
- Sheet name: `Revised`

### Custom files

```bash
python3 scripts/add_lemmas.py <tagged_file.xlsx> <input.xml> <output.xml>
```

### Custom sheet name

```bash
python3 scripts/add_lemmas.py --sheet "Data" <tagged_file.xlsx> <input.xml> <output.xml>
```

## Excel Format

The Excel file must contain a sheet with the following columns:

| Column | Description |
|--------|-------------|
| `id` | Token identifier (matches `id` attribute in XML `<tok>` elements) |
| `lemma` | Lemma (base form) of the token |
| `tag` | POS tag (Part-of-Speech tag) |

Example:
```
id      word    lemma       tag
w-1     Aquj    aqui        ADV
w-2     se      eu          PRO+pes:R3s
w-3     começa  começar     V:P3s
```

## XML Format

Input XML should contain `<tok>` elements with `id` attributes:

```xml
<tok id="w-1">Aquj</tok>
<tok id="w-2">se</tok>
<tok id="w-3">começa</tok>
```

Output XML will have enriched tokens:

```xml
<tok id="w-1" lemma="aqui" tag="ADV">Aquj</tok>
<tok id="w-2" lemma="eu" tag="PRO+pes:R3s">se</tok>
<tok id="w-3" lemma="começar" tag="V:P3s">começa</tok>
```

## Examples

```bash
python3 scripts/add_lemmas.py

python3 scripts/add_lemmas.py --sheet "Revised" data/custom.tagged.xlsx input.xml output.xml

python3 ./scripts/add_lemmas.py ./data/HdE_DCE.tagged.xlsx ./data/HdE_DCE.xml output/HdE_DCE.enriched.xml
```

```bash
for i in LdM MISJ quicquevult Vespasiano VMSSB_E VMSSB_G1 VSME-V VSME-W; do
echo $i
python3 ./scripts/add_lemmas.py data/$i.tagged.xlsx data/$i.xml ./output/$i.enriched.xml
done
```

## Output

The script prints progress information:
```
Loaded 154952 token annotations from data/HdE_A.tagged.xlsx#Revised
Updated 154952 tokens; wrote data/HdE_A.with_lemmas.xml
```

## License

This project is part of the CTA-corpus initiative.
