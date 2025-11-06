# Test PDF Repository

Automated pre-flight test suite for PDF signing validation.

## Directory Structure

```
test-pdfs/
├── manifest.json           # Test case definitions and metadata
├── working/                # PDFs that should sign successfully
│   ├── wire-instructions.pdf
│   ├── test-document.pdf
│   ├── simple-test.pdf
│   └── wire-instructions-signed.pdf
├── edge-cases/             # PDFs we expect to fail (with known errors)
│   └── simple-test-signed-broken.pdf
└── future/                 # PDFs we want to support later
```

## Test Cases

See `manifest.json` for complete test case definitions.

### Working PDFs (should succeed)

| File | Description | Tags |
|------|-------------|------|
| `wire-instructions.pdf` | Original POC test PDF | `poc`, `simple`, `letter` |
| `test-document.pdf` | Created with pdf-lib, has content | `pdf-lib`, `content` |
| `simple-test.pdf` | Minimal test PDF | `minimal`, `simple` |
| `wire-instructions-signed.pdf` | Pre-signed, for validation testing | `signed`, `validation` |

### Edge Cases (expected to fail)

| File | Description | Expected Error |
|------|-------------|----------------|
| `simple-test-signed-broken.pdf` | Broken signature structure | Validation failure |

## Adding New Test PDFs

1. Drop PDF into appropriate directory (`working/`, `edge-cases/`, or `future/`)
2. Update `manifest.json` with test case metadata:
   ```json
   {
     "id": "unique-test-id",
     "file": "working/your-pdf.pdf",
     "expectedStatus": "supported|experimental|unsupported",
     "properties": {
       "version": "1.4",
       "pages": 1,
       "objects": 10,
       "pageSize": "Letter|A4|Custom",
       "encrypted": false,
       "linearized": false,
       "hasSignatures": false
     },
     "expectedOutcome": "success|failure|already-signed",
     "notes": "Why this PDF is interesting",
     "tags": ["tag1", "tag2"]
   }
   ```
3. Run pre-flight tests to verify

## Running Tests

```bash
# In the app UI: Click "Run Preflight Tests" button
# Or via test runner:
npm run test:pdfs
```

## Test Outcomes

- ✅ **PASS** - PDF signed successfully, outcome matches expectation
- ❌ **FAIL** - PDF signing failed unexpectedly, or outcome doesn't match
- ⚠️ **UNEXPECTED** - PDF behavior different from manifest (update manifest or fix code)

## Manifest Schema

See `manifest.json` for full schema.

Key fields:
- `expectedStatus`: What compatibility level we expect (supported, experimental, unsupported)
- `expectedOutcome`: What should happen (success, failure, already-signed, validation-failure)
- `properties`: PDF structure details from analyzer
- `notes`: Why this test case matters
- `tags`: Searchable labels

## Future Test PDFs

As we expand compatibility, add PDFs to `future/` directory:
- Multi-page PDFs (10+, 100+ pages)
- Different page sizes (A4, Legal, Tabloid)
- PDFs with existing forms (AcroForm)
- PDFs with existing signatures (countersigning)
- Linearized PDFs (web-optimized)
- PDF 1.5-1.7 with various features
- Complex page trees
- Compressed object streams

**Don't add encrypted PDFs** - we won't support them (by design).

## Breadcrumbs 🍞

Each test case in `manifest.json` leaves breadcrumbs:
- **What** - Which PDF, what structure it has
- **Why** - Why this test case matters
- **Expected** - What should happen
- **Actual** - What actually happened (in test results)

This helps future developers understand:
1. What PDFs we've tested
2. What edge cases we know about
3. What works vs what doesn't
4. When something breaks, what changed

## Test Statistics

Current coverage:
- **Total test cases**: 5
- **Working PDFs**: 4 (should sign successfully)
- **Edge cases**: 1 (expected failures)
- **Future PDFs**: 0 (not yet supported)

Goal: **20+ test PDFs** covering common variations before production deployment.
