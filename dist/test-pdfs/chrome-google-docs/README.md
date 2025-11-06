# Chrome & Google Docs Test PDFs

**Purpose:** Test corpus for validating PDF signing with modern browser-generated PDFs

**Generated:** 2025-11-02

---

## Test Cases

### Chrome "Print to PDF"

| File | Description | Expected Features |
|------|-------------|-------------------|
| `chrome-simple-text.pdf` | Simple text page | Object streams? XRef streams? |
| `chrome-complex-layout.pdf` | Images, formatting | Compressed content |
| `chrome-multipage.pdf` | Multiple pages | Page tree structure |

### Google Docs "Download as PDF"

| File | Description | Expected Features |
|------|-------------|-------------------|
| `gdocs-simple.pdf` | Basic document | Object streams? |
| `gdocs-with-images.pdf` | Images embedded | Image XObjects |
| `gdocs-with-tables.pdf` | Tables/formatting | Complex layout |

---

## Analysis Workflow

For each PDF, we'll run:

```bash
# 1. Check PDF version
pdfinfo chrome-simple-text.pdf | grep "PDF version"

# 2. Check for object streams
grep -a "ObjStm" chrome-simple-text.pdf && echo "Has object streams" || echo "No object streams"

# 3. Check for xref streams
grep -a "XRef" chrome-simple-text.pdf && echo "Has xref streams" || echo "No xref streams"

# 4. Detailed analysis
qpdf --show-xref chrome-simple-text.pdf

# 5. Try current signing code
# (Will document which ones fail and why)
```

---

## Expected Outcomes

**Current code (string manipulation):**
- ✅ Should work if PDF 1.4, no compression
- ❌ Will fail if object streams (can't find Catalog/Pages)
- ❌ Will fail if xref streams (can't parse binary xref)

**After fork enhancements:**
- ✅ Should work for all Chrome/Google Docs PDFs
- ✅ Handles object streams via decompression
- ✅ Handles xref streams via binary parsing

---

## Analysis Results

*(Will be filled in after running tests)*

| PDF | Version | Obj Streams | XRef Streams | Current Code | Fork | Notes |
|-----|---------|-------------|--------------|--------------|------|-------|
| ... | ... | ... | ... | ... | ... | ... |

---

**Next Steps:**
1. Drop test PDFs in this directory
2. Run analysis script
3. Document findings
4. Use results to guide fork implementation
