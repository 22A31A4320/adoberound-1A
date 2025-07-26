# adoberound-1A
Solution for Adobe "Connecting the Dots" Hackathon – Round 1A


# Adobe Hackathon – "Connecting the Dots" 📄🤖

This repository contains a complete and extensible solution for the Adobe India Hackathon 2025 – “Connecting the Dots.” The challenge is centered around transforming how we interact with PDFs by extracting structure and meaning from static documents. The project is built in two parts, corresponding to Round 1A and Round 1B of the hackathon.

---

## 🧩 Challenge Overview

Adobe’s “Connecting the Dots” Hackathon focuses on making PDFs more intelligent, interactive, and useful. Instead of just displaying information, the idea is to build a system that can interpret, summarize, and surface insights from PDF documents in a structured way.

### 🔹 Round 1A: Document Structure Extraction

In this round, participants are given a raw PDF file and are tasked with building a system that automatically extracts its structure in terms of:
- Document Title
- Headings: H1, H2, and H3
- Page numbers for each heading

This outline helps machines understand how the document is organized and can be foundational for many downstream tasks such as search, summarization, and content recommendation.

### 🔹 Round 1B: Persona-Driven Document Intelligence

In Round 1B, the focus shifts from raw structural extraction to contextual relevance. Here, a user (defined by a persona and a job-to-be-done) uploads a collection of documents. The task is to identify and rank the most relevant sections for that persona’s goal. For example, an investment analyst might want to focus on R&D and financial performance in company reports, while a researcher may be interested in experimental methods and benchmarks.

---

## 🎯 Project Objectives

- Build an end-to-end pipeline for intelligent PDF processing.
- Provide a user-friendly interface using Streamlit.
- Enable CLI and Docker compatibility for offline processing.
- Extract hierarchical structure and semantically relevant content.
- Maintain performance constraints (≤10s runtime for Round 1A, ≤60s for Round 1B).

---

## 💡 Key Features

- 📑 Title & Heading Detection:
  Extract title and headings using text layout, font size, boldness, numbering, and capitalization.
  
- 🧠 Relevance Filtering (Round 1B):
  For a given persona and job goal, extract and rank document sections that match relevant keywords.

- 🖥️ Web UI:
  Built using Streamlit, allowing users to upload PDFs, configure parameters, and view outputs interactively.

- ⚙️ Docker Support:
  Fully containerized solution that works offline on AMD64 (x86_64) CPU-only machines, meeting all Adobe hackathon constraints.

---

## 🛠 Technical Approach

### 🔍 Structural Extraction (Round 1A)

We use `pdfminer.six` to analyze each page of a PDF at a low level, focusing on layout and font metadata. The extraction algorithm includes:

1. **Text Cleanup**:
   Removes excessive spacing and combines fragmented lines that are visually aligned.

2. **Font Size Normalization**:
   Average font size is calculated for each line. Headings are often larger and aligned higher on the page.

3. **Heuristic Scoring**:
   Each line is scored based on:
   - Font size relative to max size
   - Boldness (`'Bold' in fontname`)
   - Whether the text is numbered (e.g., `1.1`, `2.3.1`)
   - Whether the text is in all caps

   Based on the score:
   - H1 = very high importance
   - H2 = medium importance
   - H3 = low importance

4. **Title Detection**:
   The largest text block on page 1 is considered the title.

5. **JSON Output**:
   Outputs a file with the format:

```json
{
  "title": "Understanding AI",
  "outline": [
    { "level": "H1", "text": "Introduction", "page": 1 },
    { "level": "H2", "text": "What is AI?", "page": 2 }
  ]
}
