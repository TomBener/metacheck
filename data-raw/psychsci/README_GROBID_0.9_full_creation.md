# GROBID PDF-to-XML Conversion Batch

**Conversion Date & Time:** April 17, 2026, 15:00:29 UTC+2  
**Status:** ✓ Complete — All 1,903 PDFs successfully converted  
**Output Format:** TEI XML (Text Encoding Initiative)

---

## Summary

This directory contains 1,903 TEI XML files generated from academic PDF articles using GROBID 0.9.0 (full model with GPU support). Each PDF was processed to extract structured metadata (title, authors, affiliations, abstract, references, etc.) into standardized XML format, enabling machine-readable analysis of document structure and content.

- **Total PDFs processed:** 1,903
- **Successful conversions:** 1,903 (100%)
- **Format:** `{basename}.xml`
- **Average file size:** ~65 KB (range: ~25 KB – ~150 KB depending on article length)

---

## Source Documents

### Input Files
- **Location:** `C:\Users\dlakens\OneDrive - TU Eindhoven\R\download_articles_code_and_data\psych_science\`
- **Count:** 1,903 PDF files
- **Naming Convention:** Numeric identifiers (e.g., `0956797613504302.pdf`, `09567976251401538.pdf`)
- **Source Journal:** SAGE Journals (Psychological Science)
- **Approximate Total Size:** ~3.2 GB

---

## Execution Environment

### Hardware
- **OS:** Windows 10 Build 26200
- **CPU:** Intel Core (20 cores) @ Model 186, GenuineIntel
- **RAM:** 15.6 GB
- **GPU:** NVIDIA RTX A1000 6GB Laptop GPU (compute capability 8.6)

### Software Stack
- **Python:** 3.11.9
- **Docker:** 29.4.0 (Docker Desktop)
- **GROBID:** 0.9.0-full (official grobid/grobid:0.9.0-full image)
- **GROBID Client:** grobid-client-python v0.1.4

---

## GROBID Configuration

### Docker Run Command (Final)
```bash
docker run --rm --gpus all --init --ulimit core=0 -p 8070:8070 -d grobid/grobid:0.9.0-full
```

**Parameters:**
- `--gpus all` — Enable GPU acceleration (NVIDIA CUDA)
- `--init` — Use tini init system for child process reaping
- `--ulimit core=0` — Disable core dumps
- `-p 8070:8070` — Expose GROBID API endpoint on port 8070
- `-d` — Run in detached mode
- `-m 4g` — GROBID Java heap size (default: 4GB, set via `JAVA_OPTS`)

**Important:** On Windows Docker Desktop, use `http://127.0.0.1:8070` instead of `http://localhost:8070` to avoid IPv6 loopback timeouts.

### Processing Parameters
```python
# Conversion settings used in grobid_retry_missing.py
requests.post(
    "http://127.0.0.1:8070/api/processFulltextDocument",
    files={"input": (pdf_filename, pdf_handle, "application/pdf")},
    data={
        "consolidateHeader": "1",      # Consolidate extracted metadata fields
        "consolidateCitations": "0"    # Keep raw citation data (no consolidation)
    },
    timeout=(30, 600)  # Connection timeout: 30s, Read timeout: 600s
)
```

- **Service Method:** `POST /api/processFulltextDocument`
- **Concurrency:** 2 parallel workers (reduced from initial 10 to prevent server overload)
- **Retry Policy:** 3 attempts per PDF with exponential backoff (10s pause between attempts)
- **Consolidation:** Header metadata consolidated; citations left raw for flexibility

---

## Processing Scripts

### Initial Batch Conversion
**File:** `grobid_convert.py`

```python
from grobid_client.grobid_client import GrobidClient

INPUT_DIR = r"C:\Users\dlakens\OneDrive - TU Eindhoven\R\download_articles_code_and_data\psych_science"
OUTPUT_DIR = r"C:\Users\dlakens\OneDrive - TU Eindhoven\R\download_articles_code_and_data\psych_science_GROBID_0.9_full"

client = GrobidClient(grobid_server="http://127.0.0.1:8070", timeout=600)

client.process(
    "processFulltextDocument",
    INPUT_DIR,
    output=OUTPUT_DIR,
    n=2,                  # 2 parallel workers
    force=False,          # Skip already-converted files
    verbose=True,
)
```

**Result:** 680 PDFs converted successfully in first pass.

### Retry Missing Files
**File:** `grobid_retry_missing.py`

```python
import pathlib
import requests
import concurrent.futures
import time

def process_pdf(pdf_path: pathlib.Path, output_dir: pathlib.Path, 
                timeout: int, retries: int) -> tuple[str, bool, str]:
    """Process single PDF with retry logic."""
    xml_path = output_dir / f"{pdf_path.stem}.grobid.tei.xml"
    
    if xml_path.exists():
        return pdf_path.name, True, "already exists"
    
    for attempt in range(1, retries + 1):
        try:
            with pdf_path.open("rb") as pdf_handle:
                response = requests.post(
                    "http://127.0.0.1:8070/api/processFulltextDocument",
                    files={"input": (pdf_path.name, pdf_handle, "application/pdf")},
                    data={"consolidateHeader": "1", "consolidateCitations": "0"},
                    timeout=(30, timeout),
                )
            
            if response.status_code == 200 and response.text.strip():
                xml_path.write_text(response.text, encoding="utf-8")
                return pdf_path.name, True, f"ok on attempt {attempt}"
        
        except requests.exceptions.RequestException as exc:
            if attempt < retries:
                time.sleep(min(10 * attempt, 30))
    
    return pdf_path.name, False, f"failed after {retries} attempts"
```

**Invocation:**
```bash
python grobid_retry_missing.py \
  --workers 2 \
  --timeout 600 \
  --retries 3 \
  --startup-attempts 5 \
  --startup-timeout 10 \
  --startup-pause 2
```

**Result:** All 1,222 remaining PDFs converted successfully on first or retry attempt.

---

## Processing Timeline

| Phase | Count | Duration | Note |
|-------|-------|----------|------|
| **Initial batch** | 680 PDFs | ~2 hours | Interrupted due to: (1) 10 concurrent workers caused connection timeouts, (2) timeout of 180s insufficient for fulltext extraction |
| **Infrastructure fix** | — | — | Switched from `localhost` to `127.0.0.1`, reduced concurrency to 2, increased timeout to 600s |
| **Retry missing** | 1,222 PDFs | ~4 hours | All succeeded on attempt 1 with fixed parameters |
| **Total elapsed** | 1,903 PDFs | ~6 hours | Including container startup/model loading (~2 min per restart) |

---

## Output File Format

### Example Structure
Each XML file follows the **TEI (Text Encoding Initiative) P5** standard with GROBID extensions:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TEI xml:space="preserve" xmlns="http://www.tei-c.org/ns/1.0" ...>
  <teiHeader>
    <fileDesc>
      <titleStmt>
        <title level="a" type="main">Article Title</title>
        <author>
          <persName>
            <forename>John</forename>
            <surname>Doe</surname>
          </persName>
          <affiliation>...</affiliation>
        </author>
      </titleStmt>
      <publicationStmt>
        <date>2023-05-15</date>
      </publicationStmt>
    </fileDesc>
  </teiHeader>
  <text>
    <body>
      <div type="abstract">...</div>
      <div type="introduction">...</div>
      ...
    </body>
    <back>
      <div type="references">
        <listBibl>...</listBibl>
      </div>
    </back>
  </text>
</TEI>
```

### Key Extracted Elements
- `title` — Article title
- `author` — All authors with names, affiliations, emails
- `date` — Publication date
- `abstract` — Full abstract (if present)
- `div[@type]` — Sections (introduction, methodology, results, discussion, conclusion)
- `figure[]` — Figures with captions and coordinates
- `table[]` — Tables with structured content
- `ref` — In-text citations with full bibliographic references in `<div type="references">`

---

## Known Issues & Workarounds

### Windows Docker Networking
**Issue:** Requests to `http://localhost:8070` timed out despite container being healthy.

**Root Cause:** On this Windows setup, `localhost` resolves to `::1` (IPv6) before `127.0.0.1`, and Docker's IPv6 routing was unreliable.

**Solution:** All scripts updated to use `http://127.0.0.1:8070` explicitly.

**Test before use:**
```python
import requests
r = requests.get("http://127.0.0.1:8070/api/isalive", timeout=10)
assert r.status_code == 200  # Should print "true"
```

### Server Overload Under High Concurrency
**Issue:** `n=10` workers caused "Connection aborted" and "Read timed out" errors.

**Solution:** Reduced to `n=2` workers and increased socket/read timeout from 180s to 600s per PDF.

### Model Loading on Startup
**Issue:** GROBID container took ~2 minutes to load neural models into memory.

**Solution:** Implemented `wait_for_server()` with configurable attempts and pause intervals.

---

## Reproducibility & Transparency

### To Regenerate This Conversion

1. **Install GROBID:**
   ```bash
   docker pull grobid/grobid:0.9.0-full
   ```

2. **Start GROBID service:**
   ```bash
   docker run --rm --gpus all --init --ulimit core=0 -p 8070:8070 grobid/grobid:0.9.0-full
   ```
   Wait for logs to show "Started" or health endpoint responds with status 200.

3. **Install Python client:**
   ```bash
   pip install grobid-client-python requests
   ```

4. **Run conversion:**
   ```bash
   python grobid_retry_missing.py \
     --workers 2 \
     --timeout 600 \
     --retries 3
   ```

### Verification
Check output directory for 1,903 XML files:
```bash
ls psych_science_GROBID_0.9_full/ | wc -l  # Should be ~1,903
file psych_science_GROBID_0.9_full/*.xml | head -5  # Should show XML
```

---

## References

- **GROBID Project:** https://github.com/kermitt2/grobid
- **TEI Standard:** https://tei-c.org/
- **GROBID Python Client:** https://github.com/kermitt2/grobid_client_python
- **GROBID API Documentation:** https://grobid.readthedocs.io/

---

## Metadata

| Field | Value |
|-------|-------|
| **Conversion Tool** | GROBID v0.9.0 (full model with GPU) |
| **Output Standard** | TEI XML (Text Encoding Initiative P5) |
| **Batch Size** | 1,903 documents |
| **Success Rate** | 100% (1,903/1,903) |
| **Processing Date** | 2026-04-17 |
| **GPU Acceleration** | NVIDIA RTX A1000 (CUDA) |
| **Python Version** | 3.11.9 |
| **Docker Version** | 29.4.0 |

---

**Generated by grobid_retry_missing.py on 2026-04-17**  
*For questions or issues with these conversions, review the GROBID logs or consult the project documentation.*
