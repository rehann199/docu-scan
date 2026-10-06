# Technical Case Study

## Challenge: Legal Document Intake and Classification

Manual classification of incoming legal documents is time-consuming and error-prone. The goal was to build an intake system that extracts text from multiple document formats, intelligently categorizes by case type and priority, and surfaces results via intuitive search.

## Solution: Modular NLP Pipeline

### Text Extraction (OCR)

Tesseract OCR runs on scanned PDFs and image uploads via the `pytesseract` wrapper and `pdf2image` for page extraction. For native PDFs and Word documents, `PyPDF2` and `python-docx` extract text directly. The OCR service abstracts format detection and raises clear errors if content extraction fails.

### Entity Extraction

spaCy's pre-trained NER model (`en_core_web_sm`) identifies persons, organizations, dates, and GPEs in extracted text. Extracted entities become searchable tags and appear in document detail cards.

### Classification

Case-type and urgency classification use statistical scoring: TF-IDF vectorization of document text, keyword dictionaries for each class,. This approach is transparent (keyword-weighted) and explainable, unlike a black-box deep learning model. A simple date-based heuristic flags documents with near-term deadlines as high urgency.

### Indexing and Search

Elasticsearch 8.11 indexes documents with tokenized text, entity tags, case type, urgency level, and metadata. Full-text queries and aggregations power search results and dashboard statistics. Redis is provisioned in the Compose stack as a cache/session store.

## Key Design Decisions

**Why statistical classification over transformers:** The requirements.txt lists transformers and torch, but they are not used. Statistical classifiers (TF-IDF + keyword weighting) offer sufficient accuracy for this intake scenario, run without a GPU, and produce interpretable results. A transformer model would add complexity for an intake scenario that rule-based scoring already covers.

**Why Elasticsearch over SQL:** Document structure is semi-structured (extracted text + metadata). Elasticsearch's schema-less indexing and built-in full-text scoring eliminate impedance mismatch and simplify queries. Relational schemas would require complex joins between document metadata and entities.

**Why spaCy over custom rules:** spaCy NER handles a broad range of entity types out of the box. Custom regex rules alone would miss context-dependent entities. spaCy strikes a balance between coverage and performance.

**Frontend simplicity:** The repository includes an unused React scaffold in `frontend/src`, but the deployed frontend is vanilla HTML + JavaScript. This choice avoids a build step, reduces container size, and meets the UI needs (forms, tables, modals, charts via CDN libraries).

## Demonstrated Patterns

- **Service-oriented architecture** – OCR, NLP, document, and Elasticsearch services are decoupled and testable
- **Pydantic validation** – Typed request/response models and enum-based enumerations (CaseType, UrgencyLevel) prevent invalid state
- **Health and readiness checks** – FastAPI health, readiness and liveness endpoints
- **Batch processing** – A `/api/v1/documents/batch/process` route accepts document IDs and starts a background task
- **Export workflow** – CSV/JSON export routes with a streaming response

## Production Considerations

Demo mode uses pre-seeded fictional data and does not require Tesseract, spaCy models, or Elasticsearch to be running. The repository defines these dependencies as Docker Compose services, but the full stack could not be started locally for this portfolio (no Docker daemon). The `simple_backend.py` fallback server exposes basic Elasticsearch queries on a separate port, useful for debugging or low-overhead searches when NLP pipelines are unavailable.
