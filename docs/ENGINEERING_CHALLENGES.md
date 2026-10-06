# Engineering Challenges

## Challenge 1: Bridging Frontend and Backend Contracts

**Problem:** The static frontend expects unwrapped JSON responses (e.g., `{ "documents": [...] }`), while the FastAPI routes wrap responses in a `SuccessResponse` envelope. The fallback `simple_backend.py` uses yet another set of paths and data shapes, so the frontend and backend contracts do not line up out of the box.

**Approach:** For the demo, a small stand-in API server returns the unwrapped shapes the frontend expects, backed by seeded fictional data. In a real deployment the frontend would be updated to handle the envelope, or the contract would be standardized. The lesson: define API contracts explicitly before implementation.

## Challenge 2: Chart Rendering in Flex Containers

**Problem:** The dashboard's case-type bar chart collapses to zero height inside a flex-centered container, so the bars render empty.

**Approach:** In the demo, a small CSS rule is injected at serve time to restore the chart layout, leaving the original source tree untouched. The proper fix would be an explicit container height in the source HTML.

## Challenge 3: Multi-Format OCR

**Problem:** Documents arrive as scanned PDFs, native PDFs, Word files and images, each needing a different extraction path.

**Approach:** The OCR service combines Tesseract (via pytesseract and pdf2image) for scans and images with PyPDF2 and python-docx for native formats. Its docstring describes error handling and retry logic; this was not exercised in the demo because Tesseract is not running there.

## Challenge 4: Classification Confidence

**Problem:** Keyword and statistical classifiers can misclassify short or ambiguous documents, so users need to see how certain a result is.

**Approach:** Classification returns confidence scores that the UI surfaces as a bar on each document card. The classifier is rule-based and statistical (keyword weighting, TF-IDF), not an LLM or trained model.

## Challenge 5: Unused Dependencies

**Problem:** `requirements.txt` lists transformers and torch, but no service uses them, which inflates installs and build time.

**Approach:** Documented as a cleanup item. The classifier is statistical by design, so these packages can be removed or deferred until a transformer-based classifier is actually built.

## Lessons and Patterns

- **Make confidence explicit:** Surface classifier uncertainty to users.
- **Define API contracts early:** The envelope mismatch in Challenge 1 is a direct result of skipping this.
- **Version the API:** Routes live under `/api/v1`, with typed Pydantic models.
