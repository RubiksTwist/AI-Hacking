<!--
	OWASP LLM Top 10 — expanded and reformatted
	Purpose: provide a concise threat model for LLM-based applications.
	Edit/Review: treat this as a living document. Add project-specific notes beneath each section.
-->

1. Prompt Injection
- violate guidelines, generate harmful content, enabled unauthorized access, influence critical decisions.

- main focus and jailbreaking

- direct prompt injection
  - e.g. "ignore all previous instructions"

- indirect prompt injections
  - e.g. malicious content in tickets or scraped docs used by RAG

2. Sensitive information disclosure

- model can be coaxed to reveal secrets or PII (especially when using public models with no fine-tuning or RAG without filters)

3. Supply chain
- vulnerabilities in training data, packages, dependencies; watch for compromised model weights and malicious packages

4. Data and model poisoning
- adding poisoned examples to training/fine-tune datasets introduces backdoors or biased outputs (note: out-of-scope if you only call public models and never fine-tune)

5. Improper output handling
- insufficient validation/sanitization of model outputs
- potential impacts: XSS, CSRF, SSRF, RCE when outputs are inserted into apps or executed

6. Excessive agency
- risk if the system grants model/tooling ability to perform actions (send emails, run commands, modify data) without human checks

7. System prompt leakage
- system prompts may contain sensitive info (API keys, private instructions) and should not be treated as a secure control

8. Vector and embedding weaknesses
- RAG/indexing issues can retrieve unintended or sensitive passages; embedding collisions and cross-tenant leakage are key concerns

9. Misinformation
- hallucinations and confidently wrong answers about internal/company facts

10. Unbounded consumption
- resource/cost abuse, DoS via high-volume requests, or model extraction attacks to clone intellectual property

https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/
