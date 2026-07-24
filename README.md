# Modern AI Agent Isolation with NVIDIA OpenShell: Complete Architecture & Step-by-Step Hands-On Notebook

**NVIDIA OpenShell** is an open-source, policy-driven runtime designed specifically for autonomous, self-evolving AI agents. Unlike traditional container runtimes that isolate generic workloads, OpenShell provides **out-of-process, zero-trust environmental guardrails**.

An AI agent running inside an OpenShell sandbox can self-evolve, write new Python scripts mid-task, and install tools—yet it remains completely incapable of exfiltrating credentials, traversing unauthorized filesystem paths, or making unapproved outbound network requests.

---

## 1. Deep-Dive Architecture & Core Concepts

OpenShell adopts a security model analogous to a modern web browser's tab sandbox:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                             HOST ENVIRONMENT                                │
│                                                                             │
│  ┌───────────────────────┐             ┌─────────────────────────────────┐  │
│  │     OpenShell CLI     │             │        openshell-gateway        │  │
│  │  (`openshell sandbox`)│             │   (gRPC / HTTP Server :17670)   │  │
│  └───────────┬───────────┘             └────────────────┬────────────────┘  │
│              │                                          │                   │
│              └──────────────────┐  ┌────────────────────┘                   │
│                                 ▼  ▼                                        │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                    SANDBOX EXECUTION ENVIRONMENT                      │  │
│  │  ┌─────────────────────────────────────────────────────────────────┐  │  │
│  │  │                        Agent Harness                            │  │  │
│  │  │                 (Claude Code, Codex, Custom)                    │  │  │
│  │  └──────────────────────────────┬──────────────────────────────────┘  │  │
│  │                                 │ (Executes Commands)                 │  │
│  │                                 ▼                                     │  │
│  │  ┌─────────────────────────────────────────────────────────────────┐  │  │
│  │  │                       OUT-OF-PROCESS POLICY                     │  │  │
│  │  ├─────────────────────────────────────────────────────────────────┤  │  │
│  │  │ 1. Filesystem Layer : Linux Landlock Sandboxing                 │  │  │
│  │  │ 2. Network Layer    : L7 Proxy Filtering (Host, API, Egress)    │  │  │
│  │  │ 3. Privacy Router   : Zero-Trust LLM Credential Swapping        │  │  │
│  │  └─────────────────────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────────────────────┘  │  │
└─────────────────────────────────────────────────────────────────────────────┘

```

### Key Architectural Pillars

1. **Out-of-Process Enforcement**: Security controls live *outside* the agent's context window and process boundary. Even if an attacker executes a successful prompt injection against the agent, the kernel and OpenShell gateway refuse prohibited syscalls and network routes.
2. **Linux Landlock Filesystem Isolation**: Enforces granular directory-level read/write rules at the Linux kernel level.
3. **L7 Proxy & Network Filtering**: Outbound HTTP/HTTPS requests pass through a transparent policy proxy. Unapproved endpoints are immediately blocked with `403` status codes.
4. **Privacy Router (Credential Isolation)**: Keeps LLM provider API keys outside the sandbox entirely. The agent directs local inference requests to the internal route, and OpenShell swaps in authorized backend API credentials in-flight.
