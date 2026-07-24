# Modern AI Agent Isolation with NVIDIA OpenShell

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/irAbs174/openshell-notebook/blob/main/English.ipynb)

**NVIDIA OpenShell** is an open‑source, policy‑driven runtime designed to securely isolate autonomous, self‑evolving AI agents. This repository provides a **complete step‑by‑step Jupyter notebook** that guides you from zero to running an isolated AI agent with custom declarative security policies.

> 🇬🇧 **English** · 🇷🇺 **Русский** · 🇮🇷 **فارسی**

---

## 📖 Overview

OpenShell moves security controls **outside** the agent’s process and context window. Even if an attacker injects a malicious prompt, the kernel and OpenShell gateway enforce **zero‑trust** restrictions on filesystem access, network egress, and system calls.

This notebook demonstrates:
- Environment diagnostics and verification
- Starting the `openshell‑gateway` daemon
- Creating an isolated sandbox
- Applying dynamic YAML policies (filesystem, L7 network, process execution)
- Testing policy enforcement (blocked POST requests, blocked egress domains)
- Auditing logs and cleaning up resources
- Advanced patterns:
  - OIDC/JWT authentication for multi‑tenant production
  - Multi‑stage agent pipelines (generation vs. execution)
  - Prometheus metrics integration

---
