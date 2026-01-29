# Exploit Clone Detection Analysis

**BEP Jaime Ram - 5558581**

Identifying Semantic Similarities Between Proof-of-Concept Exploits and Known Exploited Vulnerabilities in Open-Source Software Using Type-4 Clone Detection.

---

## Overview

This project analyzes exploit code from ExploitDB to identify semantic similarities between Known Exploited Vulnerabilities (KEV) and non-KEV exploits using code clone detection techniques.

The analysis uses four types of clone detection:
- **Type-1 (Textual)**: Identical code except whitespace, layout, comments
- **Type-2 (Lexical)**: Identical except identifier names and literal values
- **Type-3 (Syntactic)**: Similar at statement level with added/modified/removed statements
- **Type-4 (Semantic)**: Different syntax but same functionality

---

## Requirements

### Python Version
- Python 3.8 or higher

### Required Packages

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy requests tree-sitter-languages vulncheck-sdk
```

Or install all at once:
```bash
pip install -r requirements.txt
```

### API Keys

| API | Purpose | How to Get |
|-----|---------|------------|
| **VulnCheck** | Access KEV catalog | Free signup at [vulncheck.com](https://vulncheck.com) |

**Setting up VulnCheck API Key:**

1. Create a free account at https://vulncheck.com
2. Navigate to API Keys in your dashboard
3. Generate a new API key
4. Set as environment variable:
   ```bash
   export VULNCHECK_API_TOKEN="your-api-key-here"
   ```
   Or paste directly in Step 2.1 of the notebook.

---

## Notebook Structure

### Part 0: Setup
| Step | Description |
|------|-------------|
| 0.1 | Install and import all required packages |

### Part 1: Data Extraction & Cleaning
| Step | Description |
|------|-------------|
| 1.1 | Extract ExploitDB data (2020-2025) |
| 1.2 | Download exploit code and extract CVEs |
| 1.3 | Combine CVEs and filter exploits |
| 1.4 | Open source filtering (OSV API) |
| 1.5 | Remove wrapper scripts (quality filter) |

### Part 2: KEV Cross-Reference
| Step | Description |
|------|-------------|
| 2.1 | Download VulnCheck KEV data |
| 2.2 | Cross-reference exploits with KEV database |

### Part 3: Clone Detection Analysis
| Step | Description |
|------|-------------|
| 3.1 | Parse exploit code (AST, normalization, Markov matrices) |
| 3.2 | Clone detection (Types 1-4) |
| 3.3 | Clone type comparison visualization |

### Part 4: Payload Analysis
| Step | Description |
|------|-------------|
| 4.1 | Payload string similarity analysis |
| 4.2 | Payload characteristic analysis |

### Part 5: Visualization & Results
| Step | Description |
|------|-------------|
| 5.1 | Similarity score distributions |
| 5.2 | High similarity pairs table |
| 5.3 | CVSS vs similarity correlation |
| 5.4 | Filtering pipeline overview |
| 5.5 | Payload comparison (side-by-side) |

---

## Data Flow

```
ExploitDB (2020-2025)
    │
    ▼
CVE Extraction & Filtering
    │
    ▼
Open Source Filtering (OSV API)
    │
    ▼
Quality Filtering (Remove Wrappers)
    │
    ▼
KEV Cross-Reference (VulnCheck)
    │
    ├──► KEV Exploits (CVE in KEV catalog)
    │
    └──► Non-KEV Exploits (CVE not in KEV catalog)
            │
            ▼
      Code Parsing (AST, Markov)
            │
            ▼
      Clone Detection (4 Types)
            │
            ▼
      Analysis & Visualization
```

---

## Output Files

| File | Description |
|------|-------------|
| `outputs/clone_detection_all_pairs.csv` | All pairwise similarity scores |
| `outputs/high_similarity_type1.csv` | High-similarity pairs (Type-1) |
| `outputs/high_similarity_type2.csv` | High-similarity pairs (Type-2) |
| `outputs/high_similarity_type3.csv` | High-similarity pairs (Type-3) |
| `outputs/high_similarity_type4.csv` | High-similarity pairs (Type-4) |
| `outputs/clone_detection_summary.json` | Statistics summary |
| `outputs/payload_similarity_results.json` | Payload analysis results |
| `outputs/figures/` | Generated visualizations |

---

## Usage

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd "BEP code - Jaime Ram - 5558581"
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up API key**
   ```bash
   export VULNCHECK_API_TOKEN="your-api-key"
   ```

4. **Run the notebook**
   ```bash
   jupyter notebook exploit_analysis.ipynb
   ```

5. **Execute cells sequentially** (Part 0 → Part 5)

---

## Clone Detection Methods

### Type-1: Textual Similarity
- Normalizes whitespace and removes comments
- Uses SequenceMatcher for character-level comparison
- Detects exact or near-exact copies

### Type-2: Lexical Similarity
- Replaces identifiers with placeholders (VAR, FUNC, etc.)
- Replaces literals with type placeholders (NUM, STR, etc.)
- Detects renamed but structurally identical code

### Type-3: Syntactic Similarity
- Extracts AST node sequences
- Uses Jaccard index for set comparison
- Detects similar code with added/removed statements

### Type-4: Semantic Similarity
- Builds Markov transition matrices from AST
- Uses weighted distance metric:
  - 40% Cosine similarity
  - 20% Euclidean distance
  - 20% Manhattan distance
  - 20% Chebyshev distance
- Detects functionally similar code with different syntax

---

## References
- Inspired by Wu et al.
- ExploitDB: https://www.exploit-db.com
- VulnCheck KEV: https://vulncheck.com
- CISA KEV Catalog: https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- OSV Database: https://osv.dev

---

## Author

**Jaime Ram** - Student ID: 5558581

Bachelor's End Project (BEP)
