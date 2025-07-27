# Adobe-India-Hackathon
PDF intelligence toolkit for Adobe India Hackathon – structure extraction and persona-based document insight ranking, fully offline.

Full Project Details (Adobe India Hackathon Submission)

# 🧠 Adobe Hackathon Project – “Connecting the Dots”
### 🔗 Reimagining PDFs with Structure Intelligence and Persona-Based Understanding

This project was developed as part of the **Adobe India Hackathon 2025 - Connecting the Dots Challenge**, which aimed to transform how we read and interact with PDFs. The two core goals are:

1. **Round 1A:** Extracting structured outlines (Title, H1, H2, H3) from raw PDF files.
2. **Round 1B:** Ranking the most relevant document sections based on a defined persona and their job-to-be-done.

The platform is designed to be **lightweight**, **offline-capable**, and **fully containerized** for reproducible evaluation.

---

## 🚀 Round 1A – PDF Outline Extraction

### 🎯 Objective:
To extract a document outline from a PDF, including:
- Title
- Section headings (H1, H2, H3)
- Page numbers
- Hierarchy of structure

### 🛠️ Approach:
Using `PyMuPDF (fitz)`, we parse the PDF and determine heading levels based on:
- Font size distribution
- Page structure
- Text position

We avoid relying solely on font size by analyzing layout patterns and filtering irrelevant text.

### 📥 Input:
- A folder named `/input/` containing `.pdf` files (≤50 pages each)

### 📤 Output:
- For each PDF, a JSON file is created in `/output/` directory with the following format:
- json
{
  "title": "Understanding AI",
  "outline": [
    { "level": "H1", "text": "Introduction", "page": 1 },
    { "level": "H2", "text": "What is AI?", "page": 2 },
    { "level": "H3", "text": "History of AI", "page": 3 }
  ]
}


### 🧠 Round 1B – Persona-Based Document Insight Extraction

### 🎯 Objective:
To simulate how a user (persona) interacts with a collection of documents by extracting only the most relevant sections based on their role and goal.


### 🧑 Persona Example:
{
  "persona": "PhD Researcher in Computational Biology",
  "job_to_be_done": "Prepare a literature review on methodologies and benchmarks in drug discovery"
}

### 🛠️ Approach:
We use TF-IDF (via scikit-learn) and cosine similarity to compute the relevance of each page/section of the documents with respect to the persona’s objective. No large ML models or internet access is needed, making the solution lightweight and offline-ready.

### 📥 Input:
/input/ folder with 3–10 related PDF documents

persona.json file describing persona and their task

### 📤 Output:
A structured file persona_analysis.json in the /output/ directory containing:

Metadata: list of input documents, persona, job, and timestamp

Top 5 most relevant sections

Refined text from each section


### Sample output:

{
  "metadata": {
    "documents": ["doc1.pdf", "doc2.pdf"],
    "persona": "PhD Researcher",
    "job_to_be_done": "Literature review on GNNs",
    "timestamp": "2025-07-21T12:30:00Z"
  },
  "sections": [
    {
      "document": "doc1.pdf",
      "page": 4,
      "section_title": "GNN Methodologies Overview...",
      "importance_rank": 1
    }
  ],
  "sub_sections": [
    {
      "document": "doc1.pdf",
      "page": 4,
      "refined_text": "This section discusses different GNN models...",
      "importance_rank": 1
    }
  ]
}


### 📦 Tech Stack
Python 3.10+

PyMuPDF (fitz) – for PDF parsing

scikit-learn – for TF-IDF vectorization and cosine similarity

No GPU or internet required

Docker-ready & AMD64 compliant


### 🧪 Input/Output Summary
Phase	Input	Output
Round 1A	PDF file in input/	Outline JSON in output/
Round 1B	PDFs in input/, persona.json	Ranked results in output/

### 🐳 Docker Compatibility
This project is fully compatible with Docker as required by Adobe. Your image can be built and run using:

docker build --platform linux/amd64 -t mysolution:latest .
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output --network none mysolution:latest



### 📜 Final Notes
The system supports multilingual PDFs if text is extractable (Unicode-based).

Works offline, suitable for air-gapped environments.

No large models — stays within memory/time constraints.

### 👨‍💻 Author
Rishabh Singh
B.Tech Student, NIET Greater Noida
GitHub: github.com/rishabh01032003
