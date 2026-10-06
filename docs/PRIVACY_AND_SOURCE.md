# Privacy and Source

## Data Handling

Docu-Scan is designed for legal documents, which are sensitive by nature. This portfolio contains only fictional data.

- **Authentication:** The backend includes JWT authentication routes (python-jose/passlib, HTTP Bearer). The demo build runs without authentication.
- **Logging:** The backend uses Loguru for application logging. The data model includes an optional `user_id` field on an action record. No audit-trail, encryption, or access-control guarantees are claimed here.
- **Demo data:** All names, clients, documents and case identifiers shown in screenshots are fictional and generated for demonstration. No real personal or client information is included.

## Source Code Availability

The source code for this application is proprietary and not included in this portfolio repository. The documentation here summarizes the architecture and design from code inspection.

## Third-Party Libraries

Docu-Scan depends on open-source libraries including FastAPI, spaCy, Elasticsearch, Tesseract, scikit-learn, Pydantic, python-jose, Tailwind CSS and FontAwesome, each under its own license. No third-party licensed content is redistributed in this portfolio.

## Secrets and Configuration

This portfolio does not contain API keys, credentials, connection strings, or internal hostnames. Sensitive configuration, where present, is excluded.
