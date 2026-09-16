# 🔒 Security Checklist & Vulnerability Audit

> **[🤖 AI AGENT INSTRUCTIONS - READ THIS FIRST]**
> This document governs the security hardening phase of the project. As an AI agent, you must act as a Rigorous Security Auditor before authorizing any deployment.
> 1. **Dynamic Generation:** Based on the technologies listed in `docs/pra-development/4-ARCHITECTURE.md` and endpoints in `docs/pra-development/6-API-CONTRACT.md`, you must define and tailor specific security checklist items for this project (do not rely solely on generic rules).
> 2. **Strict Enforcement:** Every security item below must be verified and checked off `[x]` before the app is cleared for production deployment.
> 3. **Zero Hardcoding:** Ensure zero tolerance for hardcoded API keys, secrets, or database credentials.
> 4. **Vulnerability Flagging:** If you detect a security flaw in the codebase during your review, stop immediately, log it here, and provide a secure patch.

---

## 🛡️ 1. Authentication & Authorization Security
*Verify how user identities and access permissions are managed.*

- [ ] **Token Storage:** JWT or session tokens are stored securely (e.g., using Flutter Secure Storage / Encrypted SharedPreferences on mobile, or HttpOnly secure cookies on web). Never stored in plain `SharedPreferences` or `localStorage`.
- [ ] **Token Expiry & Refresh:** Token expiration, invalidation, and refresh mechanisms are properly implemented.
- [ ] **Route Protection:** All protected backend routes and frontend screens strictly validate user authentication roles before rendering data.
- [ ] **Password Policies:** If applicable, user passwords enforce minimum length and complexity rules.
- [ ] **Environment Isolation:** All sensitive keys (`API_KEY`, database URLs, secret tokens) are stored exclusively in local `.env` files and **never** hardcoded or committed to version control.
- [ ] **Git Ignore Verification:** The `.env`, `.env.local`, and build-specific secret files are explicitly declared in `.gitignore`.
- [ ] **Password Hashing:** Passwords are stored only as salted one-way hashes using a modern adaptive algorithm (`bcrypt`, `argon2id`, or `scrypt`) with a sane work factor. Plaintext, reversible encryption, and fast hashes (`MD5`, `SHA-1`, unsalted `SHA-256`) are never used.
- [ ] **Session Security:** Session cookies are set `HttpOnly`, `Secure`, and `SameSite=Strict/Lax`; session IDs are regenerated on login and privilege change, invalidated on logout, and expire after a defined idle/absolute timeout.
- [ ] **Secure Password Reset:** Reset tokens are cryptographically random, single-use, short-lived (e.g. 15–60 min), bound to one account, invalidated after use or password change, and delivered out-of-band. The reset response is identical for existing and non-existing emails (no account enumeration), and the endpoint is rate-limited.
- [ ] **Git History Secret Scan:** The full repository history (not just the working tree) has been scanned for committed secrets with a tool such as `gitleaks`, `trufflehog`, or GitHub secret scanning. Any secret ever committed is treated as compromised: rotated at the provider and purged from history.
- [ ] **Example Template Provided:** A safe `.env.example` file containing dummy keys is provided for developers to clone without exposing real secrets.

## 🌐 2. Network & API Security
*Verify how data travels between the client and server.*

- [ ] **HTTPS Enforcement:** All network requests strictly use `HTTPS` in production. Cleartext HTTP traffic is blocked.
- [ ] **API Key Masking:** All third-party API keys (e.g., Firebase, payment gateways, LLM services) are hidden in `.env` files and injected during build time, never exposed in client-side source code.
- [ ] **Request Validation (Input):** Both client-side and server-side validate every incoming request body, query param, and header (type, length, format, allowed values) to prevent injection attacks (SQL Injection, NoSQL Injection, XSS, command injection).
- [ ] **Response Validation (Output):** API responses are schema-checked / serialized through an explicit allowlist of fields so internal-only data (password hashes, internal IDs, stack traces, other users' data) never leaks in the payload.
- [ ] **Input Sanitization (Normalize & Reject):** Untrusted input is normalized (trim, unicode/encoding normalization) and filtered against an allowlist of permitted characters/values before it reaches business logic, storage, or any interpreter. Sanitization complements validation; it never replaces it.
- [ ] **SQL/NoSQL Injection Prevention:** All database access uses parameterized queries, prepared statements, or a trusted ORM/query builder. User input is never concatenated or interpolated into SQL/NoSQL strings. Where dynamic identifiers (table/column/sort fields) are unavoidable, they are resolved through a fixed server-side allowlist.
- [ ] **XSS Prevention:** All user-controlled data is context-aware escaped on output (HTML body, attribute, JavaScript, URL, CSS). Raw-HTML sinks (`dangerouslySetInnerHTML`, `v-html`, `innerHTML`, `eval`) are avoided or fed only through a sanitizer allowlist (e.g. DOMPurify). A `Content-Security-Policy` is in place as defense in depth.
- [ ] **Debug Mode Disabled:** Production runs with debug/dev mode off (`DEBUG=false`, framework debug toolbars, verbose tracebacks, hot-reload, and dev-only middleware all disabled). Verified against the actual deployed environment variables, not just the local config file.
- [ ] **Error Responses Don't Leak Internals:** Client-facing errors return a generic message plus a correlation ID only. Stack traces, SQL/driver errors, file paths, framework/library versions, and internal hostnames are logged server-side and never returned in a response or rendered in the UI.
- [ ] **Rate Limiting:** All critical and public-facing endpoints (login, OTP, registration, password reset, search, payment) have rate-limiting / throttling to prevent brute-force and abuse.
- [ ] **CORS Policy:** `Access-Control-Allow-Origin` is restricted to an explicit allowlist of trusted domains — never `*` combined with `Access-Control-Allow-Credentials: true`. Preflight (`OPTIONS`) responses only expose the methods/headers actually needed.
- [ ] **CSRF Protection:** All state-changing endpoints (`POST`/`PUT`/`PATCH`/`DELETE`) reachable from a browser session are protected via CSRF tokens, `SameSite=Strict/Lax` cookies, or double-submit cookie pattern.
- [ ] **SSRF Prevention:** Any server-side call to a user-supplied URL (webhooks, image/link previews, file fetch) validates the target against an allowlist and blocks requests to internal/private IP ranges, `localhost`, and cloud metadata endpoints (e.g., `169.254.169.254`).
- [ ] **Path Traversal Prevention:** Any file path, filename, or upload/download parameter built from user input is sanitized and normalized (`../`, absolute paths, symlinks rejected) and resolved only within an allowlisted base directory.
- [ ] **IDOR Check (Broken Object-Level Authorization):** Every endpoint that accepts a resource ID (`/orders/:id`, `/users/:id`, etc.) verifies the authenticated user actually owns/has permission for that specific object — not just that they are logged in.
- [ ] **Admin/Internal Route Hardening:** Admin, internal, or debug routes (`/admin`, `/internal`, `/actuator`, `/debug`, API docs like Swagger) require elevated role-based auth, are not indexable, and are not reachable from the public internet unless explicitly required.
- [ ] **Endpoint Audit:** Full inventory of exposed endpoints (from `6-API-CONTRACT.md` and actual route definitions) reviewed to confirm no forgotten, leftover-debug, or undocumented endpoints are live in production.
- [ ] **Security Headers:** Production responses set `Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`/`SAMEORIGIN`, `Strict-Transport-Security`, and `Referrer-Policy`.

## 📱 3. Client-Side & Local Data Security (Mobile/Frontend)
*Verify the security posture of the application runtime.*

- [ ] **Data Caching:** Sensitive user data cached locally is encrypted or wiped upon logout.
- [ ] **SSL Pinning (Optional/High-Security):** Implemented if the project requires maximum defense against Man-in-the-Middle (MitM) attacks.
- [ ] **Code Obfuscation:** Release builds are configured for code obfuscation and minification (e.g., ProGuard/R8 enabled for Android, or Flutter obfuscate flags).
- [ ] **Debug Logs Removal:** All debug statements, console logs, and sensitive print functions are stripped out of production builds.
- [ ] **Private Source Maps:** JS/TS source maps are not published to production (disabled or uploaded privately to an error-tracking service only), so bundled app logic can't be reverse-engineered from the browser.

## 🗄️ 4. Database Security
*Verify the data store itself is not directly reachable or over-privileged.*

- [ ] **Database Not Publicly Exposed:** The database (and cache/message broker) does not listen on a public IP. Access is restricted to the application network via VPC/private subnet, firewall/security-group allowlist, or SSH tunnel. Default management ports (`5432`, `3306`, `27017`, `6379`) are closed to `0.0.0.0/0`.
- [ ] **Least-Privilege DB Permissions:** The application connects with a dedicated, non-superuser account that holds only the privileges it actually needs (typically `SELECT`/`INSERT`/`UPDATE`/`DELETE` on its own schema — not `DROP`, `GRANT`, or admin rights). Migrations use a separate, higher-privilege account that the runtime does not hold.
- [ ] **Row-Level / Tenant Isolation:** If a BaaS or multi-tenant database is used (Supabase, Firebase, etc.), Row-Level Security / security rules are enabled and tested so one user cannot read or write another user's rows. Default-deny is the baseline; anonymous/public read is explicitly justified per table.
- [ ] **Encryption At Rest & In Transit:** Database storage and backups are encrypted at rest, and the application-to-database connection enforces TLS/SSL.

## 📤 5. File Upload Security
*Verify uploaded files can't become an execution or storage attack vector.*

- [ ] **Upload Restrictions Enforced:** Uploads are limited server-side by file size, MIME type, and extension against an allowlist (never a blocklist). The declared `Content-Type` is not trusted — the real type is verified from magic bytes/content sniffing. Per-user upload count and rate are capped.
- [ ] **Malware Scanning:** Uploaded files are scanned for malware (e.g. ClamAV or the storage provider's scanning service) before being made available for download or processing. Files that fail the scan are quarantined or deleted and the event is logged.
- [ ] **Safe Storage & Serving:** Uploads are stored outside the web root (or in object storage) with server-generated random filenames, are never executable, and are served with `Content-Disposition: attachment` plus `X-Content-Type-Options: nosniff` — or from a separate domain/bucket — so a stored file can never be executed or run script in the app's origin.

## 💳 6. Business Logic & Payment Security
*Verify that server-side logic can't be bypassed or tampered with by a malicious client.*

- [ ] **Webhook Verification:** Every incoming webhook (payment gateway, n8n, third-party service) verifies the provider's signature/secret before processing; unverified webhook calls are rejected, not silently ignored.
- [ ] **Payment Server-Side Verification:** Payment status, amount, and order state are confirmed directly with the payment provider's server/API — the client's claim of "payment success" is never trusted on its own.
- [ ] **Price/Amount Anti-Tamper:** Prices, totals, discounts, and quantities are recalculated and validated server-side from the source of truth (DB/catalog); client-submitted price fields are ignored or cross-checked, never trusted directly.

## 🔑 7. Credentials & Dependency Hygiene
*Verify nothing ships with known-weak defaults or known-vulnerable dependencies.*

- [ ] **Default Passwords Changed:** All default credentials (admin panel, database, message broker, seed/demo accounts, cloud service defaults) are changed before production deployment.
- [ ] **Dependency Updates:** Third-party packages are on patched versions; `npm audit` / `pip-audit` / equivalent has been run and high/critical vulnerabilities resolved or explicitly risk-accepted.

## 🧾 8. Logging & Backup Integrity
*Verify operational data doesn't become a liability.*

- [ ] **Logs Don't Leak Secrets:** Application logs never contain passwords, tokens, API keys, full card numbers, or other PII in plaintext (masked/redacted before writing).
- [ ] **Backup Restore Tested:** A full database/application backup has been restored end-to-end in a non-production environment to confirm backups are actually usable in a disaster-recovery scenario.

## 🧪 9. Pre-Launch Live Security Testing
*Verify the checklist holds up against a real, running deployment — not just code review.*

- [ ] **Live Security Test:** Before go-live, the deployed staging/production build is exercised with a live security pass (manual pentest checklist and/or automated scanner such as OWASP ZAP) covering auth bypass, injection, CORS/CSRF, IDOR, and rate-limit enforcement.

## 📋 10. Project-Specific Custom Security Rules
*(Agent Note: Define additional security checks tailored specifically to the features outlined in `3-PRD.md`).*

- [ ] **Custom Rule 1:** [e.g., Verify that webhook payloads from n8n are authenticated via secret signatures].
- [ ] **Custom Rule 2:** [e.g., Ensure offline local database (SQLite/Hive) is encrypted if storing personal user data].

---
> **[🤖 AI AGENT INSTRUCTION - POST-AUDIT]**
> Once all items in this checklist are verified and marked as `[x]`, the AI agent must state: *"All security checks have successfully passed. The project is cleared for the deployment phase."*