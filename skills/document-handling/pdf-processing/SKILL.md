---
name: pdf-processing
description: Process PDF documents. Extract text, metadata, merge, split, and search within PDFs using CLI tools like poppler-utils (pdftotext, pdfinfo) and qpdf.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [PDF, Document, Extraction, Text, poppler-utils, qpdf]
    related_skills: []
---

# PDF Processing Skill

This skill allows the agent to process, analyze, and extract information from PDF files using standard CLI text processing tools. It is highly recommended to use `pdftotext` from the `poppler-utils` package for text extraction.

## Prerequisites

To effectively process PDFs, the host system needs standard CLI tools. Check if they are installed before proceeding. If they are not installed, you MUST ask the user to install them first, OR use `sudo apt-get update && sudo DEBIAN_FRONTEND=noninteractive apt-get install -y poppler-utils qpdf` (on Debian/Ubuntu) if authorized.

**Important Error Handling:** Never try to install packages automatically without `sudo` if you are not the root user, as it will cause the process to hang waiting for a password. Ask the user for permission or instruct them to install it.

```bash
# Check for pdftotext (poppler-utils)
command -v pdftotext || echo "pdftotext not found"

# Check for qpdf (for merging/splitting/manipulation)
command -v qpdf || echo "qpdf not found"
```

## Core Capabilities

### 1. Extracting Text from PDF

The most robust way to read a PDF is to extract its content to a text file first.

```bash
# Extract all text to a file (preserves layout and basic formatting)
pdftotext -layout document.pdf output.txt

# Read the extracted text (use head/tail/less if the file is large)
cat output.txt
```

### 2. Extracting Specific Pages

When dealing with large PDFs, extract only the pages you need to avoid context overflow.

```bash
# Extract from page 5 to page 10
pdftotext -f 5 -l 10 document.pdf output.txt
```

### 3. Reading PDF Metadata

Use `pdfinfo` to get document properties (number of pages, author, creation dates). Always do this first when encountering a new PDF!

```bash
pdfinfo document.pdf
```

### 4. Merging and Splitting PDFs (requires qpdf)

```bash
# Merge two PDFs into one
qpdf --empty --pages file1.pdf file2.pdf -- output_merged.pdf

# Split a PDF into individual pages (output_page_1.pdf, output_page_2.pdf, etc.)
qpdf --split-pages document.pdf output_page_%d.pdf
```

## Best Practices for Agents

1. **Avoid `cat` on PDFs:** Never read a binary `.pdf` file directly using `cat` or `head`. It will flood your terminal with unreadable binary data.
2. **Handle Large Files Smartly:** Always check `pdfinfo` first to see how many pages the document has. If it's over 10-20 pages, extract incrementally (e.g., `-f 1 -l 10`) instead of all at once to stay within your context window.
3. **Targeted Searching:** After converting a PDF to text, use `grep` to find specific information without reading the whole document.
   ```bash
   grep -n -C 2 "important keyword" output.txt
   ```
4. **Cleanup:** Remember to clean up your temporary text files (`output.txt`) after you have successfully extracted the information you need, to avoid cluttering the user's workspace.
