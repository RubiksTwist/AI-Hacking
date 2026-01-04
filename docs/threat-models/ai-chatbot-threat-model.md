# AI Chatbot Threat Model

## Scenario
Chatbot created for a company called **Too Many Cables**, which sells HDMI cables, charging cables, adapters, USB cables, and audio cables.

---

## Who (Threat Actors)

### Competitors
- Rival cable companies seeking competitive intelligence
- Looking to understand pricing strategies, supplier relationships, and inventory data
- May use chatbot to extract product roadmaps or exclusive supplier information

### Black Hat Hackers
- Financially motivated attackers targeting customer payment data
- Seeking to steal and sell PII on dark web markets
- May attempt credential stuffing or account takeover attacks
- Could ransom stolen data back to the company

### State-Sponsored Actors
- Advanced persistent threat (APT) groups
- Interested in supply chain intelligence and infrastructure
- May target for broader economic espionage
- Could use as entry point to larger supply chain attacks

### Script Kiddies
- Low-skill attackers using automated tools
- Testing known exploits against chatbot interface
- May cause service disruption through fuzzing or spam
- Often looking for easy wins or notoriety

---

## What (Assets)

### E-commerce System
- Product catalog and inventory management
- Shopping cart and checkout functionality
- Order processing and fulfillment systems
- Customer account management portal

### Database
- Customer records and order history
- Product inventory and pricing data
- Transaction logs and analytics
- Authentication credentials and session tokens

### PII (Personally Identifiable Information)
- Customer names, addresses, phone numbers
- Email addresses and communication preferences
- Purchase history and browsing behavior
- Account credentials (usernames, hashed passwords)

### PCI (Payment Card Industry) Data
- Credit card numbers, CVV codes, expiration dates
- Billing addresses and payment methods
- Transaction details and payment history
- Tokenized payment information

### Supplier Information
- Supplier contact details and contracts
- Pricing agreements and terms
- Delivery schedules and logistics data
- Exclusive partnership agreements

---

## What (Attack Surface)

- Input channels: any input prompts, uploaded files, or user-modifiable content processed by the model (user prompts, file attachments, or form inputs that can influence model behavior).
- Retrieval / RAG layer: the retrieval-assisted generation layer and vector store — malicious documents, poisoned embeddings, or tampered context returned to the model that cause unsafe/incorrect outputs.
- Tools / plugins: any external tools, plugins, or agent capabilities the model can call or interact with (e.g., code execution, HTTP/API calls, system commands).
- Supply chain: backdoored or malicious models, compromised libraries, or third‑party dependencies introduced during model download, deployment, or updates.
- Logging: application and model logs, telemetry, or audit trails that may contain sensitive prompts, PII, API keys, or details about model internals.
- Admin / Debug endpoints: management, debug, or metrics endpoints that expose configuration, secrets, system internals, or allow privileged actions if not properly protected.
- Training data: pre-training, fine-tuning, or augmentation data that could be poisoned or inadvertently include sensitive information that the model learns and can reveal.

---

## Impact (Risks)

### Expose PII
- Customer names, addresses, and contact information leaked
- Privacy violations leading to GDPR/CCPA fines
- Loss of customer trust and brand reputation damage
- Potential class-action lawsuits from affected customers

### Expose PCI Data
- Credit card information stolen and sold
- PCI DSS compliance violations and audit failures
- Card brand fines and increased processing fees
- Mandatory breach notifications and credit monitoring costs

### Expose Passwords
- Account credentials compromised enabling account takeover
- Credential stuffing attacks on other platforms if passwords reused
- Unauthorized purchases and fraudulent orders
- Need for forced password resets across all customers

### Expose Database Information
- Complete customer database exfiltration
- Access to business logic and system architecture
- Ability to modify or delete critical data
- Potential for ransomware or data destruction attacks

### Expose Supplier Information
- Loss of competitive advantage through leaked supplier deals
- Strained supplier relationships if contracts exposed
- Competitors poaching exclusive suppliers
- Disruption to supply chain and inventory management

### Expose Customer Data
- Shopping patterns and preferences revealed
- Targeted phishing campaigns using stolen data
- Identity theft and financial fraud
- Erosion of customer loyalty and retention
