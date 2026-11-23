# Stage 02 — Application Foundation

[Master checklist](../README.md)

## Build

### 1. Establish the development environment

- [ ] Initialize version control if still absent; do not overwrite existing research or planning files.
- [ ] Create frontend, backend, tests, and infrastructure directories following the selected architecture.
- [ ] Pin dependencies and document supported runtime versions.
- [ ] Add development configuration templates without real credentials.
- [ ] Add commands for formatting, linting, type checks, and automated tests; run the same commands in CI when configured.
- [ ] Provide reproducible local database, storage, and worker setup.

### 2. Implement persistence and access

- [ ] Add migrations for identity references, investigations, notes, jobs, and shared record metadata.
- [ ] Implement the selected authentication/session flow and server-side ownership checks.
- [ ] Support creating, listing, opening, renaming, and deleting an investigation with defined behavior for its existing records.
- [ ] Establish revision/concurrency handling so background results do not overwrite user changes.
- [ ] Preserve user edits and revisions; add reviewer-specific history in stage 05 rather than building it before claims exist.

### 3. Build the bilingual application shell

- [ ] Add Arabic/English localization, language switching, RTL layout, and mixed-direction text handling.
- [ ] Build sign-in/access screens, investigation list, and investigation shell.
- [ ] Establish reusable navigation, forms, citation placeholders, loading, empty, and error states.
- [ ] Make keyboard navigation, labels, focus, and readable contrast part of shared components.

### 4. Establish jobs and service boundaries

- [ ] Implement a consistent queued/running/succeeded/failed/cancelled state contract.
- [ ] Add authorized status retrieval and safe retry/cancellation behavior.
- [ ] Pass access scope and input versions to jobs and recheck authorization before reading private inputs.
- [ ] Define connector/model interfaces and use test doubles in automated tests.
- [ ] Add health/readiness checks and structured logs that omit private content and secrets by default.

### 5. Build baseline security

- [ ] Configure secure session handling, allowed origins, CSRF protection where applicable, and request limits.
- [ ] Keep secrets server-side and exclude local credentials, uploads, and private logs from version control.
- [ ] Ensure database, file, and job identifiers do not bypass authorization.
- [ ] Ensure deletion and access checks cover current investigation/job records; extend them to files during stage 03.

## API and Screen Checklist

Endpoint paths are examples to finalize when designing the API.

- [ ] Implement investigation list/create operations, for example `GET/POST /investigations`.
- [ ] Implement investigation detail/update/delete, for example `GET/PATCH/DELETE /investigations/{investigation_id}`.
- [ ] Implement job status and authorized retry/cancel actions, for example `GET /jobs/{job_id}`.
- [ ] Add a public health endpoint without leaking restricted diagnostic details.
- [ ] Connect investigation list, create form, language switch, workspace shell, and job-status components to the API.

## Check

- [ ] A fresh environment can start the application and apply migrations using documented commands.
- [ ] An authorized user can create an investigation and reopen it after restart.
- [ ] A second user cannot read, edit, delete, or inspect jobs belonging to the first.
- [ ] Both languages render correctly; Arabic navigation uses RTL without corrupting URLs/numbers.
- [ ] A test job reports success/failure and can be retried without duplicate side effects.
- [ ] Invalid sessions, unavailable services, and concurrent edits produce clear outcomes.
- [ ] The formatting, type, and baseline test commands pass locally and in CI if configured.

- [ ] Complete the working flow: start the app → sign in → switch language → create/reopen a private investigation.

**Next:** [Stage 03 — Content intake and provenance](03-content-intake.md)
