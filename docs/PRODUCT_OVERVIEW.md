# Product Overview

Docu-Scan is a legal document management system that automates intake and classification for law firms. It handles PDF, Word and image uploads with OCR, applies NLP-based intelligent categorization, and provides a search and analytics interface for case and document discovery.

## Core Workflow

**Intake.** Upload documents with optional case name, client identifier, and urgency hint. The system extracts text via OCR and pipelines it through NLP classification.

**Classification.** Tesseract extracts text; spaCy identifies entities (names, dates, organizations, legal terms); keyword-weighted statistical classifiers assign case type and urgency with confidence scores; extractive summarization produces a brief overview.

**Search and Access.** Full-text search across documents with filtering by case type and urgency level. Results include entity tags, confidence scores, and quick links to detail views. One-click CSV and JSON export.

**Dashboard.** Four KPI cards (total documents, high priority, case types, active clients), a bar chart of case type distribution, and an upload timeline.

## Key Features

- **Drag-and-drop upload** with metadata hints (case, client, urgency)
- **OCR pipeline** for scanned and image-based documents
- **Entity extraction and tagging** via spaCy NLP (names, dates, orgs, legal entities)
- **Statistical case-type and urgency classification** with transparent confidence scores
- **Full-text search** with case-type and urgency filters
- **Document cards** showing metadata, entities, confidence bars, and extracted summary
- **Export** of search results in CSV and JSON formats
- **Static UI** built with Tailwind CSS via CDN

## Technical Stack

- **Backend:** FastAPI with Pydantic models, JWT authentication (python-jose/passlib), health/metrics endpoints
- **Search:** Elasticsearch 8.11 for indexing and full-text queries
- **NLP:** spaCy entity extraction, TF-IDF and keyword-based classification (scikit-learn)
- **OCR:** Tesseract via pytesseract and pdf2image
- **Infrastructure:** Docker Compose orchestration (Elasticsearch, Redis, backend, nginx-served frontend, reverse proxy)
- **Frontend:** Vanilla JavaScript with Tailwind CSS via CDN; static HTML served by nginx

The demo environment uses pre-seeded fictional data and does not require OCR, NLP or Elasticsearch libraries to be installed locally.
