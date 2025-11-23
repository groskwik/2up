# nup_pdf.py
### Multi-PDF N-Up Generator (2-up / 4-up, vector-safe)

`nup_pdf.py` is a powerful, production-ready tool for generating **2-up** and **4-up** PDF layouts without rasterization. It is designed for printing manuals, duplicating pages, placing multiple PDFs on one sheet, and preparing documents for booklet-style cutting.

The program uses **pypdf** and preserves full **vector content**—text remains searchable, graphics remain crisp, and file sizes stay small.

---

## ✨ Features

- **2-up layout** (left/right on landscape sheet)
- **4-up layout** (2×2 grid)
- **Vector-safe**: no rasterizing of PDF content  
- **Automatic scaling** with optional `--zoom` factor
- **Different PDFs in each slot** (manual mode)
- **Automatic blank-slot padding** so page numbers stay synchronized  
- **Two workflow modes**:
  - **Default mode** — search for a PDF by partial name inside `PDF_FOLDERS`
  - **Manual mode** — specify one or more PDFs directly or select them interactively
- **Supports Letter and A4**, portrait or landscape
- **Margins and gutters** (`--margin-in`, `--gutter-in`, `--gutter-y-in`)
- Fully documented command-line interface

---

## 📂 Default Workflow (No Arguments)

Running:

```
python nup_pdf.py
```

Performs the following:

1. Asks the user:
   ```
   Enter part of the PDF filename:
   ```
2. Searches across all directories listed in `PDF_FOLDERS`:
   ```python
   PDF_FOLDERS = [
       r"C:\Users\benoi\Downloads\ebay_manuals",
       r"C:\Users\benoi\Downloads\manuals",
   ]
   ```
3. Finds all matching PDFs (case-insensitive substring match)
4. Allows selection if multiple matches exist
5. Uses that single PDF as the **only source**  
   → 2-up duplicates left & right  
   → 4-up duplicates all four quadrants

This mode is optimized for your daily manual-production workflow.

---

## 🔧 Manual Input Mode

To bypass the automatic search and specify PDFs manually:

```
python nup_pdf.py --manual-inputs
```

You may then:

### Provide PDFs directly:
```
python nup_pdf.py left.pdf right.pdf --mode 2up --manual-inputs
```

or

### Let the script list the PDFs in the current directory:
```
python nup_pdf.py --manual-inputs
```

The script will prompt you with a numbered list.

---

## 🧪 Examples

### 2-up duplicate (default)
```
python nup_pdf.py
```

### 2-up with zoom (shrink content 10%)
```
python nup_pdf.py --zoom 0.9
```

### 2-up with two different PDFs (manual)
```
python nup_pdf.py left.pdf right.pdf --mode 2up --manual-inputs
```

### 4-up with four different PDFs
```
python nup_pdf.py a.pdf b.pdf c.pdf d.pdf --mode 4up --manual-inputs
```

### 4-up with two PDFs
```
python nup_pdf.py A.pdf B.pdf --mode 4up --manual-inputs
```

### 4-up with margins and gutters
```
python nup_pdf.py --mode 4up --margin-in 0.25 --gutter-in 0.15 --gutter-y-in 0.15
```

---

## 🚀 Installation

Install pypdf:

```
pip install pypdf
```

You’re ready to go.

---

## 📄 Command-Line Arguments

| Argument | Description |
|---------|-------------|
| `inputs...` | Input PDFs (only used with `--manual-inputs`). |
| `--mode {2up,4up}` | Select layout type (default: `2up`). |
| `--manual-inputs` | Use positional PDFs / CWD selection rather than folder search. |
| `--zoom FLOAT` | Shrink content (≤1.0) or allow limited enlargement (>1.0 capped). |
| `--margin-in FLOAT` | Outer margin in inches. |
| `--gutter-in FLOAT` | Horizontal gutter between columns. |
| `--gutter-y-in FLOAT` | Vertical gutter (4-up only). |
| `--sheet {letter,a4}` | Select sheet size. |
| `--orientation {portrait,landscape}` | For 4-up only. |
| `--align {top,center,bottom}` | Vertical alignment inside slots. |
| `--stop {longest,shortest}` | Handle mismatched page counts. |
| `-o OUTPUT.pdf` | Output filename. |

---

## 📌 Handling PDFs With Different Page Counts

When mixing PDFs:

- If `--stop longest` (default):  
  Shorter PDFs are **padded with blank pages**  
  → Page numbers remain synchronized across all sources.

- If `--stop shortest`:  
  Processing stops at the smallest page count.

This ensures predictable layout behavior for booklet-style cuts.

---

## 🧱 Internal Layout Logic

- 2-up always forces **landscape** orientation.
- 4-up supports **portrait or landscape**.
- Scaling is **uniform** (never distorted).  
- Zoom is applied after fit, but never exceeds slot boundaries.
- Vector contents are preserved using:
  ```
  merge_transformed_page()
  ```

No rasterization. No quality loss.

---

## 🔍 Search Logic (Default Mode)

When no `--manual-inputs` flag is used:

- The script requests a **partial filename**
- Looks through **PDF_FOLDERS**
- Performs a **case-insensitive substring match**

If multiple PDFs match:
→ user chooses from a numbered list.

If only one match:
→ it is used automatically.

If none found:
→ script exits with a friendly message.

---

## 🛠 Development Notes

- Fully Python 3.8+ compatible.
- Pure pypdf: zero external native dependencies.
- Easy to extend:  
  - booklet mode  
  - 6-up or 8-up modes  
  - binding-safe inner margins  
  - bleed/crop marks  
  - batch directory processing
- Designed for high-volume manual printing operations.

---

## 📬 Support and Extensions

If you'd like enhancements such as:

- Booklet-ready signatures  
- Duplex-aware layouts  
- Auto-rotate to maximize slot coverage  
- Zero-margin printer compensation  
- Integration with your eBay printing pipeline  
- GUI front-end  
- Batch folder automation  

Just ask — the script is modular and easy to extend.

