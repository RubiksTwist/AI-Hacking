 # Rules of Engagement (RoE) — TCM AI Chatbot
 **Version:** 2025-10-22

 Short summary
 - Concise RoE for authorized security testing of the TMC AI chatbot (Flask + Ollama + ChromaDB). Keep tests within the in-scope APIs and constraints below.

 ## 1. Defined scope

 In-scope (TMC chatbot-focused)

 - Core API endpoints (chat & conversation):
	 ```
	 POST /api/chat
	 GET /api/conversation/<id>
	 POST /api/conversation/<id>/clear
	 GET /api/conversations
	 ```
 - Authentication: `POST /api/login`, `POST /api/register`, `GET /api/user`, `POST /api/logout` (session-based + CSRF)
 - Support ticket system:
	 ```
	 POST /api/tickets/create
	 GET /api/tickets/<ticket_number>
	 POST /api/tickets/<id>/update
	 GET /api/tickets/user
	 ```
 - RAG / Knowledge base (admin operations): `GET /api/knowledge-base/stats`, `POST /api/knowledge-base/search`, `POST /api/knowledge-base/reindex`
 - Vector DB (ChromaDB): semantic search, retrieval testing via application interfaces only
 - Autonomous AI agent: ticket decision-making flows (auto-close, escalate, discounts)
 - Admin interfaces and system health endpoints (see Admin section)

 Out-of-scope (unless expressly authorized in writing)

 - Physical hosts, Docker host OS, hardware attacks
 - Docker Compose, launch/stop scripts, or container filesystem modifications
 - Direct ChromaDB/SQLite file manipulation (only via app APIs)
 - Social engineering of staff
 - Network-level attacks against host
 - Tests that intentionally cause production service degradation (DoS)

 Note: launch infra-as-code is provided for tester-controlled environments only; not part of production testing.

 ## 2. Allowed tests & constraints

 Permitted (low → medium risk)

 - Passive recon: enumerate Flask endpoints, client-side JS, parameters
 - Active functional probing: chat messages, API mutations, conversation handling
 - Prompt injection/jailbreak testing across AI security levels (1–5)
 - Auth testing: session manipulation, CSRF checks, role escalation
 - RAG tests: knowledge-base disclosures, vector-search manipulation
 - AI agent manipulation: influence ticket decisions via conversations
 - Limited model extraction: controlled Q&A up to query caps (no bulk exfil)

 High-risk tests (development environment only, with explicit approval)

 - ChromaDB index poisoning, bulk data extraction, production data deletion
 - Distributed DoS, filesystem access beyond API scope

 ## 3. Admin interfaces & sensitive endpoints

 Admin endpoints in-scope (authorization/privilege testing):

 ```
 /api/admin/tickets
 /api/admin/tickets/<id>/assign
 /api/admin/tickets/<id>/status
 /api/admin/tickets/<id>/reply
 /api/knowledge-base/reindex
 /api/knowledge-base/search
 ```

 Sensitive knowledge-base documents may be tested for disclosure via RAG as realistic attack scenarios.

 ## 4. Test data & environment

 - Vector DB: use existing ChromaDB collections via app APIs; do not modify local DB files.
 - Synthetic canaries: `CANARY_TMC_[INITIALS]_[YYYYMMDD]` (e.g., `CANARY_TMC_AB_20251022`) for membership inference tests.
 - Test accounts (pre-provisioned):
	 - Customer: `customer@example.com / customer123`
	 - Admin: `admin@toomanycables.com / admin123`
 - Create synthetic tickets using test customer accounts for AI agent manipulation tests.

 ## 5. Rate limits & load constraints

 Apply these default caps unless working in a dedicated dev environment (request required to exceed):

 - Chat API: max 30 requests / minute
 - Login attempts: max 5 requests / minute
 - General API: max 100 requests / hour
 - Admin endpoints: max 200 requests / hour
 - Vector searches: max 50 queries / hour
 - Model extraction queries: max 200 queries / day

 Exceeding caps requires written client approval.

 ## 6. Data handling, sensitive findings & remediation

 If sensitive credentials/secrets are discovered (API keys, passwords, certs):

 1. Document exact reproduction steps and evidence.
 2. Notify TMC security contact immediately if credentials appear production-valid.
 3. Do NOT use found credentials to access external services.
 4. Include findings and remediation recommendations in the security report.

 Evidence & retention

 - Store test artifacts encrypted and share securely with TMC. No public disclosure without consent.
 - Retain evidence for 90 days after final report, then securely delete test data.

 ## 7. Logging, monitoring & alerts

 - Flask app logs: auth attempts, API calls, security events — available for test correlation.
 - Health endpoints: `/api/health`, `/api/health/ollama` for live status checks.
 - Rate-limiting may temporarily block tests — expected behavior.
 - AI security levels ≥2 log blocked attempts for validation.
 - Docker/container health checks monitor Flask and Ollama availability.

 ## 8. Emergency escalation & pause/stop

 - Client security: `security@toomanycables.local`
 - Client ops: `ops@toomanycables.local`

 Immediate pause/stop conditions (Client may issue):

 - Material service instability or availability impact
 - Risk to production data or accidental exposure of production secrets
 - Tester must pause if tests cause broad unexpected impact or on Ops request

 Pause/stop process: email security contact; Tester must acknowledge within 2 hours (business hours) or 4 hours otherwise.

 ## 9. Evidence & reporting deliverables

 - Daily status updates via email during active testing
 - Draft technical report: within 5 business days after test completion (exec summary, findings, PoCs, remediation)
 - Final report: updated after client feedback (one revision included)
 - Retest verification: agreed timeline after remediation

 Report contents: exec summary, reproducible PoCs (sanitized), logs (secure), remediation recommendations, estimated effort to exploit.

 ## 10. Legal & liability

 - Tester acts as independent contractor; testing authorized per signed RoE.
 - Tester will not perform illegal acts or intentional data destruction beyond the RoE.
 - Both parties keep findings confidential; public disclosure requires mutual written consent.

 ## 11. Acceptance criteria & success metrics (examples)

 - AI security bypass: all configured levels (2–5) resist specified jailbreak attempts (validated by sample tests)
 - Harmful output filtering: security levels prevent generation of harmful/off-topic content across sample prompts
 - RAG disclosure: sensitive docs in `/knowledge_base/development/` not retrievable by standard users
 - Admin auth: customer accounts cannot access `/api/admin/*` routes
 - AI agent robustness: autonomous decisions not manipulable to perform harmful actions

 ## 12. System architecture & deployment context (summary)

 - App: Flask web app with AI chatbot
 - AI: Ollama local LLM (Mistral variants)
 - Vector DB: ChromaDB for RAG
 - Session store: SQLite (server-side sessions)
 - Knowledge base: markdown docs in `/knowledge_base/`
 - Deployment: Dockerized; Ollama default port 11434 (configurable)
 - Security levels: 1 (no filters) → 5 (full I/O filtering)

 ## 13. Contacts & sign-off

 By signing, TMC authorizes the Tester to perform the security assessment within the stated scope.

 Tester (name, signature): _________________________  Date: __________

 TMC Client (name, signature): _____________________  Date: __________

 TMC Tech Lead (name, signature): __________________  Date: __________

 ---
 Provided by TCM (AI-Hacking Course)
