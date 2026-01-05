# AI-Hacking

This repository demonstrates a local AI chatbot setup that runs an Ollama server on a Windows host and a chatbot application inside a Linux VM (container). The chatbot communicates with Ollama via HTTP to fetch and use local models (e.g., IBM Granite 3B, Mistral 7B).

## Architecture

The following ASCII diagram shows the components and how they interact:

```
┌─────────────────────────────────────────────────────────────────┐
│                     HOST WINDOWS COMPUTER                        │
│                                                                  │
│  ┌────────────────────────────────────────────────────────┐    │
│  │                    OLLAMA SERVER                        │    │
│  │                                                         │    │
│  │  ┌──────────────────┐      ┌──────────────────┐       │    │
│  │  │  IBM Granite 3B  │      │   Mistral 7B     │       │    │
│  │  │ (Apache 2.0)     │      │  (Apache 2.0)    │       │    │
│  │  └──────────────────┘      └──────────────────┘       │    │
│  │                                                         │    │
│  │           Listening on: localhost:11434                │    │
│  └────────────────────────────────────────────────────────┘    │
│                              ▲                                  │
│                              │                                  │
│                              │ HTTP API Calls                   │
│                              │                                  │
│  ┌───────────────────────────┼──────────────────────────────┐  │
│  │           DOCKER          │                              │  │
│  │                           │                              │  │
│  │  ┌────────────────────────┼───────────────────────────┐ │  │
│  │  │      LINUX VM          │                           │ │  │
│  │  │                        │                           │ │  │
│  │  │  ┌─────────────────────▼────────────────────────┐ │ │  │
│  │  │  │         AI CHATBOT APPLICATION              │ │ │  │
│  │  │  │                                             │ │ │  │
│  │  │  │  - Fetches models from Ollama              │ │ │  │
│  │  │  │  - Processes user queries                  │ │ │  │
│  │  │  │  - Returns AI responses                    │ │ │  │
│  │  │  └─────────────────────────────────────────────┘ │ │  │
│  │  │                                                   │ │  │
│  │  └───────────────────────────────────────────────────┘ │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Data flow

User Query → Linux VM Chatbot → Ollama (Windows Host)
                                 ↓
                  Process with Granite 3B or Mistral 7B
                                 ↓
User Response ← Linux VM Chatbot ← Ollama Response

## Getting started (high level)

1. Run Ollama on the Windows host and ensure it listens on localhost:11434.
2. Start the Linux VM/container (Docker) that runs the AI chatbot application.
3. Configure the chatbot to call Ollama's HTTP API at http://host.docker.internal:11434 (or the appropriate host address) to access models.
4. Send queries to the chatbot; it will fetch model responses from Ollama and return them to the user.

## Installing Burp Suite Community Edition (Ubuntu VM)

To perform security testing on the chatbot application, install Burp Suite Community Edition in the Ubuntu VM:

https://portswigger.net/burp/communitydownload

## Notes

- Models used in this example (IBM Granite 3B, Mistral 7B) are under Apache 2.0 in this setup; validate licensing for your use case.
- Adjust host addressing depending on your VM/container networking (e.g., host.docker.internal, bridge, or mapped ports).

---

## Repository Structure

This repository contains AI security testing tools, documentation, and results from various attack techniques.

### [TMC_Chatbot](TMC_Chatbot/)

Testing against the Too Many Cables customer service chatbot application.

#### [Reconnaissance](TMC_Chatbot/Recon/)

Initial reconnaissance and probing of the chatbot system.

- [Notes.md](TMC_Chatbot/Recon/Notes.md) - Detailed reconnaissance findings and observations
- [temperature_probe.py](TMC_Chatbot/Recon/temperature_probe.py) - Script to test model temperature variations
- [rate_limit_tester.py](TMC_Chatbot/Recon/rate_limit_tester.py) - Script to test API rate limiting
- [probe_results_sequential_20260104_175200.csv](TMC_Chatbot/Recon/probe_results_sequential_20260104_175200.csv) - Sequential probe test results

#### [Prompt Injection & Jailbreak](TMC_Chatbot/Prompt_inject&Jailbreak/)

Comprehensive testing of prompt injection and jailbreak techniques.

- [PI_notes.md](TMC_Chatbot/Prompt_inject&Jailbreak/PI_notes.md) - Complete notes on all prompt injection attempts and findings

**Results:**
- [DI_results.csv](TMC_Chatbot/Prompt_inject&Jailbreak/Results/DI_results.csv) - Direct Injection attack results
- [NI_results.csv](TMC_Chatbot/Prompt_inject&Jailbreak/Results/NI_results.csv) - No Inspection attack results
- [OEI_results.csv](TMC_Chatbot/Prompt_inject&Jailbreak/Results/OEI_results.csv) - Output Encoding Injection results
- [parseltong_results.csv](TMC_Chatbot/Prompt_inject&Jailbreak/Results/parseltong_results.csv) - P4RS3LT0NGV3 mutation technique results
- [jailbreak_results.csv](TMC_Chatbot/Prompt_inject&Jailbreak/Results/jailbreak_results.csv) - Jailbreak technique results (Do Anything Now, Opposite Mode, Rewrite Guidelines)

### [Tools](tools/)

Security testing tools and utilities.

#### [LLMmap](tools/LLMmap/)

LLM vulnerability scanning and mapping tool for automated security assessment of AI systems.

### [Documentation](docs/)

Reference materials and frameworks for AI security testing.

- [frameworks/](docs/frameworks/) - Security testing frameworks and methodologies
- [threat-models/](docs/threat-models/) - AI/ML threat modeling documentation

---
