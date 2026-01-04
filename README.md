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

## Notes

- Models used in this example (IBM Granite 3B, Mistral 7B) are under Apache 2.0 in this setup; validate licensing for your use case.
- Adjust host addressing depending on your VM/container networking (e.g., host.docker.internal, bridge, or mapped ports).

---
