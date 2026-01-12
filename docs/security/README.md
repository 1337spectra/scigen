# Security Reference Documentation

This directory contains official OWASP security guidelines and best practices that inform the SCIgen Python rewrite project.

## Documents

### 1. OWASP LLM Top 10 (LLMAll_en-US_FINAL.pdf)
**Size**: 8.4 MB
**Relevance**: ⭐⭐⭐⭐⭐ (Critical)

The comprehensive OWASP Top 10 for Large Language Model Applications. This is our primary security reference for Phase 7 (AI Integration).

**Key Topics**:
- LLM01: Prompt Injection (critical for our AI features)
- LLM02: Insecure Output Handling
- LLM03: Training Data Poisoning
- LLM04: Model Denial of Service
- LLM05: Supply Chain Vulnerabilities
- LLM06: Sensitive Information Disclosure
- LLM07: Insecure Plugin Design
- LLM08: Excessive Agency
- LLM09: Overreliance
- LLM10: Model Theft

**Our Implementation**: See `CLAUDE.md` and `PLAN.md` Phase 7 for how we address each vulnerability.

---

### 2. OWASP Top 10 for Agentic Applications 2026 (OWASP-Top-10-for-Agentic-Applications-2026-12.6-1.pdf)
**Size**: 1.3 MB
**Relevance**: ⭐⭐⭐⭐⭐ (Critical)

**Brand new** guidance for AI agent applications (December 2026 release). Highly relevant for our interactive mode and future agent-based features.

**Key Topics**:
- Security considerations for autonomous agents
- Agent-to-agent communication security
- Tool use and function calling security
- Multi-step reasoning vulnerabilities
- Agent memory and context handling

**Our Implementation**: Critical for Phase 7.7 (Interactive Mode) and any future autonomous agent features.

---

### 3. OWASP GenAI COMPASS RunBook (OWASP-GenAI-COMPASS-RunBook-1.0.pdf)
**Size**: 1.6 MB
**Relevance**: ⭐⭐⭐⭐ (High)

Operational guide for securing generative AI applications in production.

**Key Topics**:
- Risk assessment frameworks
- Security testing procedures
- Incident response for AI systems
- Compliance and governance
- Monitoring and logging

**Our Implementation**: Informs Phase 4 (Testing & QA) and Phase 5 (CI/CD) security practices.

---

### 4. OWASP GenAI Security Project Solutions Reference Guide (OWASP-GenAI-Security-Project-Solutions-Reference-Guide-Q2_Q325.pdf)
**Size**: 3.5 MB
**Relevance**: ⭐⭐⭐⭐ (High)

Comprehensive solutions and mitigation strategies for GenAI security threats.

**Key Topics**:
- Technical controls and countermeasures
- Architecture patterns for secure AI
- Code examples and implementation guidance
- Testing methodologies
- Tool recommendations

**Our Implementation**: Referenced throughout all phases, particularly Phase 1 (Input Validation), Phase 2 (Output Sanitization), and Phase 7 (AI Integration).

---

### 5. CheatSheet: Securely Using Third-Party MCP Servers (ChearSheet-A-Practical-Guide-for-Securely-Using-third-party-MCP-Servers1.0.pdf)
**Size**: 981 KB
**Relevance**: ⭐⭐⭐ (Medium)

Practical guide for securing Model Context Protocol (MCP) server integrations.

**Key Topics**:
- MCP server authentication
- Data validation for MCP inputs/outputs
- Network security for MCP connections
- Permission management
- Auditing and logging

**Our Implementation**: Relevant if we integrate with external MCP servers in Phase 6 (Web Interface) or Phase 7 (AI Integration). Currently lower priority but good reference for future extensibility.

---

## How to Use These Documents

### During Development
1. **Phase 0 (Architecture)**: Review all documents to inform security architecture
2. **Phase 1-3 (Core Development)**: Reference LLM Top 10 and Solutions Guide for input validation and output sanitization
3. **Phase 4 (Testing)**: Use COMPASS RunBook for security testing procedures
4. **Phase 7 (AI Integration)**: Deep dive into LLM Top 10 and Agentic Applications Top 10

### Security Reviews
Before each milestone, review the relevant sections of these documents to ensure compliance.

### Threat Modeling
Use these documents to identify and document threats in our threat model (Phase 0.4).

---

## Online Resources

While we have local copies, the official online versions may have updates:

- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OWASP GenAI Security Project](https://owasp.org/www-project-genai-security/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)

---

## Document Versions

All documents downloaded: **January 12, 2026**

Check online resources periodically for updates, especially:
- Security advisories
- New vulnerability patterns
- Updated mitigation strategies
- Tool recommendations

---

**Note**: These documents are copyrighted by OWASP and provided under open licensing for educational and reference purposes. See individual documents for specific licensing terms.
