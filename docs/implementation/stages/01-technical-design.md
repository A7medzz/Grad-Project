# Stage 01 — Application Design

[Master checklist](../README.md)

## Design the Application

### 1. Select the stack and project structure

- [ ] Confirm the frontend, backend, database, private file storage, and background-job tools.
- [ ] Define directories for the web application, API, worker, migrations, tests, and shared configuration.
- [ ] Define how the web application calls the backend and how the worker uses the same services.
- [ ] Choose one authentication/session approach; define investigation ownership and private-file access.

### 2. Design the initial database

- [ ] Define users/identity references, investigations, documents, document versions, passages, files, and jobs.
- [ ] Define primary keys, relationships, ownership, timestamps, and processing states.
- [ ] Specify the source metadata and page/location references required for citations.
- [ ] Define how document updates and deletions affect passages and future derived results.
- [ ] Reserve clear relationships for claims, evidence, and analysis runs; design their full tables when those features are built.

### 3. Define the first APIs and job contract

- [ ] Define requests and responses for creating, listing, opening, and updating investigations.
- [ ] Define text, URL, and PDF submission contracts and input limits.
- [ ] Define document metadata, passage retrieval, and authorized original-file access.
- [ ] Define job states, progress, errors, retry, and cancellation.
- [ ] Define authorization errors, validation errors, pagination, and revision/concurrency handling.

### 4. Design the first screens

- [ ] Define the Arabic/English application layout and RTL behavior.
- [ ] Sketch sign-in/access, investigation list, and investigation workspace.
- [ ] Sketch text/URL/PDF submission, processing status, and document viewer.
- [ ] Define source metadata and clickable page/passage references.
- [ ] Include loading, empty, unsupported-input, failed-job, and access-denied states.

### 5. Define intake and replaceable integrations

- [ ] Define the intake flow: validate → extract → preserve references → deduplicate → prepare passages.
- [ ] Select sample Arabic/English material and text-based PDFs that may be used for development.
- [ ] Identify the first permitted article/source access route; do not assume every URL is fetchable.
- [ ] Define connector inputs, outputs, source restrictions, and failure handling.
- [ ] Define model-adapter inputs and outputs without selecting every model now.
- [ ] Keep uploaded content private and decide whether any initial external service may receive it.

Reaction-source checks can run alongside the core build. Unavailable social search must not block application setup or document intake.

## Check the Design Is Usable

- [ ] The initial tables, endpoints, and screens are specific enough to implement the first workflow.
- [ ] Document ownership and access checks are defined across API, files, and jobs.
- [ ] Every extracted passage can retain its source and location for later citations.
- [ ] Input errors and job failures have defined UI/API outcomes.
- [ ] Application foundation work can begin without waiting for later model, reaction-source, or hosting choices.

- [ ] Trace the first workflow through the planned tables, APIs, and screens: create investigation → submit text/URL/PDF → process → inspect extracted text and source/page references.

**Next:** [Stage 02 — Application foundation](02-application-foundation.md)
