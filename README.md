# hardened-llm-security-gateway
 A secure Python MLOps proxy gateway designed to intercept prompt injections, shield sensitive parameters, and secure cloud-native LLM endpoints against OWASP Top 10 for LLM vulnerabilities

# Hardened LLM Security Gateway & AI Firewall

## 🎯 Project Overview
This repository hosts a production-ready **AI Security Proxy Middleware Layer** written in Python. It acts as an inline defensive gateway shielding containerized Large Language Model (LLM) applications from exploitation, input manipulation, and system prompt leakage.

## 🛡️ Threat Model & Security Controls
*   **Prompt Injection (LLM01):** Dual-layer regex and heuristic sanitization arrays that scan user input for runtime instructions bypass hooks before payload reaching the underlying model.
*   **Sensitive Data Disclosure (LLM06):** Outbound payload filtering rules that scan LLM responses for patterns resembling PII, tokens, or system configurations.
*   **Insecure Output Handling (LLM02):** Escaping and validation engines designed to nullify XSS and command execution payloads returned by foreign model tokens.

## 🛠️ Tech Stack & Lab Components
*   **Application Core:** Python (FastAPI/Flask Frameworks), Docker Containers
*   **MLOps Infrastructure:** AWS ECS (Fargate) or Azure Container Apps
*   **Defense Engine:** Custom semantic input filtering scripts and OpenWAPP rulesets
