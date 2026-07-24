Modern AI Agent Isolation with NVIDIA OpenShell: Complete Architecture & Step-by-Step Hands-On Notebook
NVIDIA OpenShell is an open-source, policy-driven runtime designed specifically for autonomous, self-evolving AI agents. Unlike traditional container runtimes that isolate generic workloads, OpenShell provides out-of-process, zero-trust environmental guardrails.

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
