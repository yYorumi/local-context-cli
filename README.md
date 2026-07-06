# local-context-cli
A local, zero-knowledge security wrapper and context-proxy for AI CLI agents with Obsidian integration.
# Local-Context-CLI (RFC / Concept)

> **Concept & Idea by:** [yYorumi](https://github.com/yYorumi)  
> **Status:** Request for Comments (RFC) / Looking for Core JS/TS Developers  
> **License:** MIT (100% Open Source & Free)

A local, zero-knowledge security wrapper and context-proxy for AI CLI agents. It builds a persistent "digital twin" of the user in **Obsidian** using lightweight Markdown metrics, preventing context bloat and protecting network privacy.

Why This Project Exists (The Backstory)

This idea was born out of pure frustration with modern AI CLI agents. 

Every time you open a new terminal session, the AI has complete amnesia. It forgets your coding style, your tech stack, your OS, and your personal preferences. You are forced to waste time explaining the same things over and over again. 

Existing solutions either try to save the entire chat history (which quickly bloats the context window, slows down the model, and costs too many tokens) or they store your user profile on closed corporate servers. 

We need a system where the AI instantly knows our context, but where **we** own 100% of our data locally.


The Solution: The Wrapper Approach

**Local-Context-CLI** reverses the traditional architecture. Instead of running a tracking utility *inside* the AI, **the AI agent runs completely isolated inside our local utility**. 

The AI lives in a "clean sandbox," believing it talks directly to the user. Meanwhile, our utility acts as a silent proxy, capturing telemetry, updating your local **Obsidian Vault**, and seamlessly injecting highly compressed, lightweight context into the session.

flowchart TD
    User([User Input: 'opencode', etc.]) --> Wrapper

    subgraph Wrapper [1. Local-Context-CLI Environment - The Wrapper]
        direction TB
        A[Reads compressed MD profile from Vault] --> B[Spawns isolated VPN tunnel]
    end

    Wrapper --> Sandbox

    subgraph Sandbox [2. Isolated AI Agent Sandbox]
        direction TB
        C[Receives micro-injected context] --> D[Agent thinks it talks directly to user]
    end

    Sandbox --> Telemetry[Silent Telemetry Parsing]
    Telemetry --> Obsidian[(Updates .md stats in Obsidian Vault)]

    %% Styling for better dark mode visibility
    style Wrapper fill:#1f2937,stroke:#3b82f6,stroke-width:2px,color:#ffffff
    style Sandbox fill:#1f2937,stroke:#10b981,stroke-width:2px,color:#ffffff
    style Obsidian fill:#2e1065,stroke:#8b5cf6,stroke-width:2px,color:#ffffff
    style A fill:#374151,stroke:#4b5563,color:#ffffff
    style B fill:#374151,stroke:#4b5563,color:#ffffff
    style C fill:#374151,stroke:#4b5563,color:#ffffff
    style D fill:#374151,stroke:#4b5563,color:#ffffff
    style Telemetry fill:#111827,stroke:#374151,color:#ffffff

Key Features

  Token-Saving Context Injection: Automatically injects small, structural user preferences (e.g., "Prefers TypeScript, hates verbose comments") derived from Obsidian data, saving thousands of tokens.

  Isolated VPN Wrapper / Kill Switch: Supply your own .conf or .ovpn file. The utility creates an isolated network tunnel strictly for the AI agent process. Your main OS internet remains untouched,
  while the AI's real IP is completely hidden. If the VPN drops, a built-in Kill Switch instantly cuts the AI's connection.

  User Data Sovereignty: Your statistics, coding habits, correction rates, and probabilities are saved as plain Markdown text with Frontmatter. You own your data, and you can edit your AI profile
  manually at any time.

Proposed Usage (Conceptual)

  # 1. Initialize and link your Obsidian Vault
  context-cli init --vault "./my-vault"

  # 2. Run any AI CLI agent inside the secure wrapper
  context-cli track "aider"

💻 Proposed Tech Stack

We are looking for Core JavaScript / TypeScript Node.js developers to lead the technical implementation.

   Runtime: Node.js / Bun

   CLI Engine: commander, inquirer, or blessed for rich terminal rendering.

   Process Interception: Node.js child_process or node-pty (pseudo-terminals) to seamlessly capture stdin/stdout streaming data without delay.

   Network Isolation: Routing the spawned process traffic via local SOCKS5/HTTP proxies or WireGuard network namespaces.

Open Questions & Architectural Dilemmas (RFC)

As this is a conceptual stage, we are actively debating:

   Context Injection Method: Should we intercept terminal streams via PTY, or act as a local API Middleware Proxy (intercepting requests to OpenAI/Anthropic endpoints)?

   Cross-Platform Network Isolation: Network namespaces work great on Linux, but what is the cleanest, sudo-free way to isolate a process network on macOS and Windows?

 Contribution & How to Help

  This project is currently in the Conceptual/Architectural stage.

If you are a developer interested in privacy, local LLMs, and the Obsidian ecosystem, we need your brain!

   Open an Issue with your thoughts on the architecture.

   Jump into the Discussions to help define the MVP scope.

   Share this RFC with fellow Node.js/TS core engineers.
