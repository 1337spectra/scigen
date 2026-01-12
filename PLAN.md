# SCIgen Python Rewrite Plan

**Created**: 2026-01-12
**Updated**: 2026-01-12
**Status**: Planning Phase → Python Rewrite
**Branch**: `claude/init-archived-project-2Tefy`
**Approach**: **Security-First Python Rewrite with OWASP Compliance**

## Overview

This document provides an atomic, actionable plan to **rewrite SCIgen from Perl to Python** with security as a first-class requirement. Rather than modernizing 21-year-old Perl code, we're building a sustainable, secure foundation suitable for long-term maintenance and AI enhancement.

## Strategic Decision: Python Rewrite

**Why Rewrite Instead of Modernize?**
- **Sustainability**: Python has a larger, more active community
- **AI Ecosystem**: Superior LLM libraries (OpenAI SDK, Anthropic SDK, LangChain)
- **Modern Tooling**: Better testing (pytest), packaging (Poetry), type hints
- **Long-term Viability**: Python is a top-tier language unlikely to decline
- **Clean Start**: Build security in from day one, not bolt it on later

**What We're Preserving**:
- Original grammar files (.in files) - they're language-agnostic
- Core grammar expansion algorithm (CFG with weighted rules)
- Seed-based reproducibility
- Command-line interface patterns
- Original behavior and output quality

---

## Security Philosophy (OWASP Compliance)

**Every phase integrates security requirements**. We follow:
- **OWASP Top 10** (web application security)
- **OWASP Top 10 for LLM Applications** (AI-specific threats)
- **Security by Design** (not an afterthought)
- **Defense in Depth** (multiple validation layers)
- **Principle of Least Privilege** (minimal permissions)

Key Security Principles:
1. **Input Validation**: Pydantic models for ALL user inputs
2. **Output Sanitization**: Escape LaTeX, HTML, shell commands
3. **Secrets Management**: Environment variables, never hardcoded
4. **Dependency Security**: Automated scanning and updates
5. **Prompt Injection Prevention**: Critical for AI features
6. **Fail Securely**: Default deny, explicit allow

---

## Phase 0: Architecture & Planning

**Goal**: Design the Python architecture and establish development foundations

### 0.1 Perl Codebase Analysis
- [ ] Deep dive into `scigen.pm` grammar engine
- [ ] Document all grammar rule features (weighted, non-duplicate, counters)
- [ ] Analyze rule expansion algorithm (recursion, randomization)
- [ ] Document pretty_print/formatting logic
- [ ] Map all Perl dependencies to Python equivalents
- [ ] Identify security issues in original Perl code
- [ ] Create comparison test suite (Perl vs Python outputs)

### 0.2 Python Architecture Design
- [ ] Design module structure (`scigen/`, `scigen/core/`, `scigen/generators/`)
- [ ] Define Pydantic models for configuration
- [ ] Design Pydantic models for input validation
- [ ] Choose CLI framework (Click vs Typer)
- [ ] Choose plotting library (matplotlib vs plotly)
- [ ] Design plugin architecture for extensibility
- [ ] Create architecture diagram (modules, data flow)

### 0.3 Project Setup
- [ ] Create Python project with Poetry (`poetry init`)
- [ ] Set up project structure (src layout)
- [ ] Configure Poetry dependencies (core, dev, optional)
- [ ] Set up pytest with coverage
- [ ] Configure mypy for strict type checking
- [ ] Configure ruff for linting and formatting
- [ ] Set up pre-commit hooks
- [ ] Create `.gitignore` for Python (`.venv/`, `__pycache__/`, etc.)

### 0.4 Security Infrastructure
- [ ] Add Safety for dependency scanning
- [ ] Add Bandit for SAST (security linting)
- [ ] Configure python-dotenv for secrets
- [ ] Create SECURITY.md with vulnerability reporting
- [ ] Set up security testing framework
- [ ] Create OWASP compliance checklist
- [ ] Document threat model

### 0.5 Documentation Foundation
- [x] Update CLAUDE.md with Python rewrite decision
- [x] Update PLAN.md (this file) with atomic tasks
- [ ] Create GRAMMAR.md documenting rule syntax
- [ ] Create CONTRIBUTING.md with development guidelines
- [ ] Create CHANGELOG.md for version tracking
- [ ] Set up documentation framework (Sphinx or MkDocs)

---

## Phase 1: Core Grammar Engine

**Goal**: Port the context-free grammar expansion engine to Python with security

**Critical Path**: This is the foundation - everything depends on it

### 1.1 Grammar File Parser
- [ ] Design GrammarRule data class (Pydantic model)
- [ ] Implement grammar file reader (`.in` file format)
- [ ] **Security**: Validate file paths (prevent directory traversal)
- [ ] **Security**: Limit file size (prevent DoS)
- [ ] Parse basic rules (RULE_NAME with options)
- [ ] Parse weighted rules (`RULE+N` syntax)
- [ ] Parse non-duplicate rules (`RULE!` syntax)
- [ ] Parse sequential counters (`RULE+` syntax)
- [ ] Parse random selection (`RULE#` syntax)
- [ ] Parse `.include` directives
- [ ] Handle circular dependency detection
- [ ] Unit tests for parser (100+ test cases)

### 1.2 Rule Expansion Engine
- [ ] Implement recursive rule expansion
- [ ] Implement weighted random selection
- [ ] Implement non-duplicate tracking
- [ ] Implement sequential counters
- [ ] Implement random selection mode
- [ ] Handle nested rule references
- [ ] **Security**: Limit recursion depth (prevent stack overflow)
- [ ] **Security**: Timeout long expansions (prevent DoS)
- [ ] Unit tests against Perl reference outputs
- [ ] Performance benchmarks

### 1.3 Text Formatting & Prettification
- [ ] Implement capitalization logic
- [ ] Implement LaTeX cleanup (spacing, punctuation)
- [ ] **Security**: Sanitize LaTeX special characters (%, $, \, {, })
- [ ] Handle abbreviations and acronyms
- [ ] Format citations and references
- [ ] Unit tests for formatting edge cases

### 1.4 Core API Design
- [ ] Create `SCIgen` main class
- [ ] Implement `load_grammar(file_path)` method
- [ ] Implement `expand_rule(rule_name, seed)` method
- [ ] Implement `generate_paper(config)` method
- [ ] **Security**: Input validation for all methods (Pydantic)
- [ ] Thread-safety considerations
- [ ] Integration tests
- [ ] API documentation (docstrings + type hints)

---

## Phase 2: Paper Generation

**Goal**: Generate complete LaTeX papers matching original Perl output

### 2.1 LaTeX Template System
- [ ] Design template structure (Jinja2 vs string.Template)
- [ ] Port main paper template
- [ ] Port bibliography template
- [ ] Port figure/table templates
- [ ] **Security**: Safe variable substitution (no eval/exec)
- [ ] **Security**: Escape all user inputs in LaTeX
- [ ] Support multiple LaTeX document classes
- [ ] Template inheritance for customization

### 2.2 Paper Structure Generation
- [ ] Generate title from grammar
- [ ] Generate abstract (SCI_ABSTRACT rule)
- [ ] Generate introduction
- [ ] Generate methodology/implementation sections
- [ ] Generate evaluation/results sections
- [ ] Generate related work
- [ ] Generate conclusion
- [ ] Maintain consistent narrative (context tracking)
- [ ] Integration tests (compare to Perl outputs)

### 2.3 Bibliography Generation
- [ ] Generate author names (non-duplicate)
- [ ] Generate paper titles
- [ ] Generate conference/journal names
- [ ] Generate publication years
- [ ] Generate citation keys
- [ ] Format BibTeX entries
- [ ] **Security**: Sanitize bibliography fields
- [ ] Fix original bugs (duplicate authors, name mismatches)
- [ ] Regression tests for bug fixes

### 2.4 LaTeX Compilation Pipeline
- [ ] Detect available LaTeX distribution (pdflatex, lualatex)
- [ ] Run pdflatex (with proper subprocess handling)
- [ ] Run bibtex for bibliography
- [ ] Run pdflatex again (2 passes for references)
- [ ] **Security**: Sanitize file paths
- [ ] **Security**: Validate LaTeX command injection
- [ ] Error handling for compilation failures
- [ ] Parse LaTeX logs for meaningful errors
- [ ] Timeout protection

### 2.5 Command-Line Interface
- [ ] Implement CLI with Click/Typer
- [ ] Add `--author` flag (multiple authors supported)
- [ ] Add `--seed` flag (reproducibility)
- [ ] Add `--file` flag (output path)
- [ ] Add `--savedir` flag (save source)
- [ ] Add `--sysname` flag (custom system name)
- [ ] Add `--help` with examples
- [ ] Add `--version` flag
- [ ] **Security**: Validate all CLI inputs (Pydantic)
- [ ] Rich output (progress bars, colors)
- [ ] Comprehensive help text

---

## Phase 3: Graphics & Diagrams

**Goal**: Generate scientific figures using Python libraries

### 3.1 Graph Generation (matplotlib)
- [ ] Port scatter plot generation
- [ ] Port line graph generation
- [ ] Port bar chart generation
- [ ] Port CDF (cumulative distribution) plots
- [ ] Generate random mathematical functions
- [ ] Add realistic noise to data
- [ ] Support error bars
- [ ] Export to EPS/PDF/PNG
- [ ] Match original aesthetic
- [ ] Unit tests for each graph type

### 3.2 Network Diagram Generation
- [ ] Integrate Graphviz Python bindings
- [ ] Generate directed graphs
- [ ] Generate undirected graphs
- [ ] Support various node shapes
- [ ] Support edge labels
- [ ] Use grammar for diagram structure
- [ ] Export to multiple formats
- [ ] Visual regression tests

### 3.3 Figure Integration
- [ ] Embed figures in LaTeX automatically
- [ ] Generate figure captions from grammar
- [ ] Number figures sequentially
- [ ] Reference figures in text
- [ ] Handle figure placement
- [ ] Support multiple figures per paper
- [ ] Test figure compilation in LaTeX

---

## Phase 4: Testing & Quality Assurance

**Goal**: Comprehensive testing with security focus

### 4.1 Unit Testing
- [ ] Test grammar parser (all rule types)
- [ ] Test rule expansion (randomization, weights)
- [ ] Test formatting logic
- [ ] Test LaTeX generation
- [ ] Test bibliography generation
- [ ] Test figure generation
- [ ] **Security**: Test input validation (fuzzing)
- [ ] **Security**: Test injection attempts (LaTeX, shell)
- [ ] Achieve >90% code coverage
- [ ] Parametrized tests with multiple seeds

### 4.2 Integration Testing
- [ ] Test full paper generation pipeline
- [ ] Test LaTeX compilation end-to-end
- [ ] Test with different LaTeX distributions
- [ ] Test on different operating systems
- [ ] Test edge cases (long names, special characters)
- [ ] Test error conditions gracefully
- [ ] Performance tests (generation speed)

### 4.3 Regression Testing
- [ ] Compare Python outputs to Perl outputs
- [ ] Verify paper structure consistency
- [ ] Verify seed reproducibility
- [ ] Test bug fixes (duplicate authors, etc.)
- [ ] Generate 100+ papers for smoke testing
- [ ] Visual diff on generated PDFs

### 4.4 Security Testing
- [ ] **Security**: Prompt injection test suite
- [ ] **Security**: LaTeX injection attempts
- [ ] **Security**: File path traversal attempts
- [ ] **Security**: Command injection attempts
- [ ] **Security**: DoS attempts (large files, deep recursion)
- [ ] **Security**: Dependency vulnerability scanning
- [ ] **Security**: SAST with Bandit
- [ ] **Security**: DAST (Dynamic testing)
- [ ] Penetration testing checklist

### 4.5 Type Checking & Linting
- [ ] Run mypy in strict mode (no Any types)
- [ ] Fix all type errors
- [ ] Run ruff linter
- [ ] Fix all linting issues
- [ ] Enforce with pre-commit hooks
- [ ] Check in CI pipeline

---

## Phase 5: Packaging & Distribution

**Goal**: Make installation and deployment easy

### 5.1 Python Package
- [ ] Configure Poetry for PyPI publishing
- [ ] Create entry points for CLI commands
- [ ] Include grammar files in package data
- [ ] Include LaTeX templates in package data
- [ ] Write comprehensive README for PyPI
- [ ] Add classifiers and keywords
- [ ] Test installation via `pip install scigen`
- [ ] Publish to Test PyPI first
- [ ] Publish to PyPI

### 5.2 Docker Container
- [ ] Create Dockerfile (multi-stage build)
- [ ] Base image: Python 3.11+ slim
- [ ] Install LaTeX distribution (minimal)
- [ ] Install Python dependencies
- [ ] Copy grammar files and templates
- [ ] Configure entry point
- [ ] Test: `docker build -t scigen .`
- [ ] Test: `docker run scigen --author "Test"`
- [ ] Optimize image size (<500MB)
- [ ] Push to Docker Hub
- [ ] Document Docker usage

### 5.3 CI/CD Pipeline
- [ ] Create `.github/workflows/test.yml`
- [ ] CI: Run pytest on Ubuntu
- [ ] CI: Run pytest on macOS
- [ ] CI: Run pytest on Windows
- [ ] CI: Run mypy type checking
- [ ] CI: Run ruff linting
- [ ] CI: Security scan with Safety
- [ ] CI: Security scan with Bandit
- [ ] CI: Dependency audit
- [ ] CI: Build Docker image
- [ ] CI: Test Docker image
- [ ] CD: Auto-publish to PyPI on tag
- [ ] CD: Auto-publish Docker on tag
- [ ] Add status badges to README

### 5.4 Documentation
- [ ] Set up documentation site (MkDocs or Sphinx)
- [ ] Write installation guide
- [ ] Write usage guide with examples
- [ ] Document CLI options
- [ ] Document Python API
- [ ] Write developer guide
- [ ] Document grammar rule syntax
- [ ] Add tutorials and examples
- [ ] Deploy docs (GitHub Pages or Read the Docs)

### 5.5 Release Management
- [ ] Set up semantic versioning
- [ ] Create CHANGELOG.md
- [ ] Write release checklist
- [ ] Create v2.0.0 tag (first Python release)
- [ ] Write release notes
- [ ] Create GitHub release
- [ ] Announce on relevant channels

---

## Phase 6: Enhanced Features

**Goal**: Add modern functionality beyond original Perl version

### 6.1 Multiple Output Formats
- [ ] Add PDF output (default, via pdflatex)
- [ ] Add HTML output (via pandoc or custom)
- [ ] Add Markdown output (simplified)
- [ ] Add DOCX output (via pandoc)
- [ ] Add `--format` CLI flag
- [ ] Test all formats
- [ ] Document format options

### 6.2 Configuration System
- [ ] Support `~/.scigenrc` (TOML/YAML)
- [ ] Support project `.scigen.conf`
- [ ] Support environment variables
- [ ] Add `--config` CLI flag
- [ ] **Security**: Validate all config values
- [ ] Provide example configs
- [ ] Document configuration schema

### 6.3 Extended Grammar Support
- [ ] Create `rules/` directory structure
- [ ] Keep original CS rules (backward compatible)
- [ ] Add biology grammar rules (`rules/biology.in`)
- [ ] Add physics grammar rules (`rules/physics.in`)
- [ ] Add `--field` CLI flag (cs, bio, physics)
- [ ] Test multi-domain generation
- [ ] Document grammar extension

### 6.4 Additional Content Types
- [ ] Generate theorems (structured gibberish)
- [ ] Generate proofs
- [ ] Generate data tables
- [ ] Generate equations (LaTeX math mode)
- [ ] Generate algorithm pseudocode
- [ ] Generate experimental results
- [ ] Integrate into paper structure
- [ ] Test all content types

### 6.5 Web Interface
- [ ] Choose web framework (FastAPI recommended)
- [ ] Design REST API
- [ ] **Security**: Add authentication (API keys)
- [ ] **Security**: Add rate limiting
- [ ] **Security**: Add CORS configuration
- [ ] Implement paper generation endpoint
- [ ] Implement status/health endpoints
- [ ] Create simple HTML frontend
- [ ] Add file download functionality
- [ ] Deploy (Docker Compose)
- [ ] Document API with OpenAPI/Swagger

### 6.6 Citation Style Support
- [ ] Research citation formats (IEEE, APA, MLA, Chicago)
- [ ] Create BibTeX style files or use existing
- [ ] Add `--cite-style` flag
- [ ] Test each citation style
- [ ] Document supported styles

---

## Phase 7: AI Integration (LLM Features)

**Goal**: Enhance SCIgen with LLM capabilities (security-first)

### 7.1 LLM Infrastructure
- [ ] Add anthropic SDK (optional dependency)
- [ ] Add openai SDK (optional dependency)
- [ ] Add langchain (optional dependency)
- [ ] **Security**: Environment-based API key management
- [ ] **Security**: Never log API keys
- [ ] **Security**: Token usage limits
- [ ] **Security**: Cost tracking and alerts
- [ ] **Security**: Timeout protection
- [ ] Design LLM abstraction layer (provider-agnostic)
- [ ] Error handling for API failures

### 7.2 Prompt Injection Prevention
- [ ] **Security**: Design secure prompt templates
- [ ] **Security**: Input sanitization for prompts
- [ ] **Security**: Use structured prompts (never concatenate user input)
- [ ] **Security**: Implement prompt validation
- [ ] **Security**: Test suite for injection attacks
- [ ] **Security**: Monitor for suspicious patterns
- [ ] Document security considerations
- [ ] Example: `f"Generate abstract for: {sanitized_input}"` not `f"{user_input}"`

### 7.3 LLM-Enhanced Content
- [ ] Add `--ai-mode` flag
- [ ] Generate coherent abstracts with LLM
- [ ] Generate coherent introductions with LLM
- [ ] Keep methodology as gibberish (preserve original)
- [ ] Add coherence slider (0-100%)
- [ ] **Security**: Sanitize LLM outputs for LaTeX
- [ ] **Security**: Validate LLM response structure
- [ ] Test quality at different coherence levels
- [ ] Compare speed (classic vs AI-enhanced)
- [ ] Document AI usage and limitations

### 7.4 Grammar Rule Generation with AI
- [ ] Design prompts for vocabulary generation
- [ ] Expand system_names.in with LLM
- [ ] Add emerging tech terms (blockchain, ML, quantum)
- [ ] Validate generated rules
- [ ] Create tool: `scigen-expand-grammar --ai`
- [ ] **Security**: Review AI-generated rules before including
- [ ] Document AI grammar expansion

### 7.5 Style Transfer
- [ ] Collect corpus of academic papers
- [ ] Extract stylistic features
- [ ] Design style transfer prompts
- [ ] Add `--mimic` flag
- [ ] Test mimicry (Knuth, Dijkstra styles)
- [ ] **Security**: Validate style inputs
- [ ] Add ethical use disclaimer
- [ ] Document style transfer

### 7.6 SCIgen Detection System
- [ ] Collect training data (real vs SCIgen papers)
- [ ] Train classifier (scikit-learn or simple heuristics)
- [ ] Create `scigen-detect` CLI command
- [ ] Add detection API endpoint
- [ ] **Security**: Rate limit detection API
- [ ] Test on historical accepted SCIgen papers
- [ ] Document accuracy and limitations
- [ ] Consider separate package

### 7.7 Interactive Mode
- [ ] Add `--interactive` CLI flag
- [ ] Prompt for topic preferences
- [ ] Preview sections before generation
- [ ] Allow section regeneration
- [ ] Save preferences
- [ ] **Security**: Validate all interactive inputs
- [ ] Rich terminal UI (with prompts)

### 7.8 Peer Review Generation
- [ ] Generate fake reviews from grammar
- [ ] Add LLM-enhanced reviews (optional)
- [ ] Include accept/reject decisions
- [ ] Generate author responses
- [ ] Simulate full review cycle
- [ ] Add `--with-reviews` flag
- [ ] Document review generation

---

## Security Checklist (OWASP Compliance)

### OWASP Top 10 Implementation

- [ ] **A01 Broken Access Control**: Rate limiting, authentication for web API
- [ ] **A02 Cryptographic Failures**: Secrets in env vars, encrypted at rest
- [ ] **A03 Injection**: Input validation (Pydantic), output sanitization, no eval/exec
- [ ] **A04 Insecure Design**: Threat modeling, secure defaults, fail securely
- [ ] **A05 Security Misconfiguration**: Secure configs, minimal dependencies
- [ ] **A06 Vulnerable Components**: Automated scanning (Safety, Dependabot)
- [ ] **A07 Auth Failures**: Strong auth for web UI, session management
- [ ] **A08 Integrity Failures**: Signed releases, dependency checksums
- [ ] **A09 Logging**: Security event logging, privacy-compliant
- [ ] **A10 SSRF**: URL validation, allowlists

### OWASP LLM Top 10 Implementation

- [ ] **LLM01 Prompt Injection**: Structured prompts, input sanitization
- [ ] **LLM02 Insecure Output**: Output sanitization, validation
- [ ] **LLM03 Training Poisoning**: N/A (using APIs)
- [ ] **LLM04 Model DoS**: Token limits, throttling, timeouts
- [ ] **LLM05 Supply Chain**: Official SDKs only, version pinning
- [ ] **LLM06 Info Disclosure**: API key protection, no logging secrets
- [ ] **LLM07 Plugin Design**: Sandbox plugins, minimal permissions
- [ ] **LLM08 Excessive Agency**: LLM text-only, human confirmation
- [ ] **LLM09 Overreliance**: Disclaimers, human review
- [ ] **LLM10 Model Theft**: API key protection

### Security Testing

- [ ] Fuzzing for input validation
- [ ] Injection attack testing (LaTeX, shell, prompt)
- [ ] DoS testing (large inputs, deep recursion)
- [ ] Dependency vulnerability scanning
- [ ] SAST (Static Application Security Testing)
- [ ] DAST (Dynamic Application Security Testing)
- [ ] Penetration testing
- [ ] Security code review

---

## Milestones & Success Criteria

### Milestone 0: "Architecture Ready" (Phase 0)
**Criteria**:
- Python project structure created
- Development tooling configured (pytest, mypy, ruff)
- Security infrastructure set up (Safety, Bandit)
- Architecture documented

### Milestone 1: "Grammar Engine Working" (Phase 1)
**Criteria**:
- Grammar parser complete
- Rule expansion matches Perl outputs
- Unit tests passing (>90% coverage)
- Security tests passing (injection, fuzzing)

### Milestone 2: "Feature Parity" (Phase 2-3)
**Criteria**:
- Generate complete papers matching Perl
- LaTeX compilation working
- Figures and diagrams generated
- All original bugs fixed
- Integration tests passing

### Milestone 3: "Production Ready" (Phase 4-5)
**Criteria**:
- PyPI package published
- Docker image available
- CI/CD pipeline operational
- Documentation complete
- Security scans clean

### Milestone 4: "Enhanced" (Phase 6)
**Criteria**:
- Multiple output formats supported
- Web interface deployed
- Extended grammar (bio, physics)
- Configuration system working

### Milestone 5: "AI-Powered" (Phase 7)
**Criteria**:
- LLM integration functional
- Prompt injection prevention validated
- Detection system operational
- Interactive mode available

---

## Timeline Estimates

**Note**: Estimates for single developer, part-time (10-15 hrs/week)

- **Phase 0**: 1 week (Architecture & Planning)
- **Phase 1**: 2-3 weeks (Core Grammar Engine - critical path)
- **Phase 2**: 2-3 weeks (Paper Generation)
- **Phase 3**: 1-2 weeks (Graphics & Diagrams)
- **Phase 4**: 1-2 weeks (Testing & QA)
- **Phase 5**: 1 week (Packaging & Distribution)
- **Phase 6**: 2-3 weeks (Enhanced Features)
- **Phase 7**: 3-4 weeks (AI Integration)

**Total**: 13-19 weeks (3-5 months part-time)

---

## Getting Started

### Immediate Next Steps (Phase 0)

1. **Study Perl Code** (3-4 hours)
   - Read `scigen.pm` thoroughly
   - Trace rule expansion logic
   - Document all features

2. **Set Up Python Project** (1-2 hours)
   ```bash
   poetry new scigen-python
   cd scigen-python
   poetry add click pydantic pytest mypy ruff
   poetry add --group dev pytest-cov bandit safety
   ```

3. **Create Project Structure** (1 hour)
   ```
   scigen-python/
   ├── src/scigen/
   │   ├── __init__.py
   │   ├── core/          # Grammar engine
   │   ├── generators/    # Paper, graph, diagram
   │   ├── cli/          # Command-line interface
   │   └── utils/        # Utilities
   ├── tests/
   ├── docs/
   ├── grammar/          # Copy .in files
   └── pyproject.toml
   ```

4. **Configure Tools** (1 hour)
   - Set up pytest configuration
   - Configure mypy (strict mode)
   - Configure ruff
   - Set up pre-commit hooks

5. **Start Phase 1** (ongoing)
   - Begin grammar parser implementation
   - Write tests as you go (TDD)

### Quick Wins (Early Momentum)

- [ ] Parse simple grammar rules
- [ ] Implement basic rule expansion
- [ ] Generate a title (SCI_TITLE rule)
- [ ] Match Perl output for one rule
- [ ] First passing test

---

## Open Questions & Decisions

### Architecture Decisions
- [ ] Click vs Typer for CLI? (Recommendation: Typer for better type support)
- [ ] Jinja2 vs string.Template for LaTeX? (Recommendation: Jinja2 for power)
- [ ] matplotlib vs plotly? (Recommendation: matplotlib for simplicity)
- [ ] FastAPI vs Flask for web? (Recommendation: FastAPI for async + OpenAPI)

### Feature Decisions
- [ ] Support Python 3.11+ only, or support 3.9+? (Recommendation: 3.11+)
- [ ] Make AI features optional or core? (Recommendation: optional extras)
- [ ] Maintain 100% Perl output compatibility? (Recommendation: Close, but don't sacrifice for it)
- [ ] Plugin system for custom generators? (Recommendation: Phase 8+)

### Distribution Decisions
- [ ] Package name on PyPI? (Check availability: scigen, scigen-py, scigen2)
- [ ] License? (Keep GPL-2.0 for compatibility with original)
- [ ] Versioning? (Start at v2.0.0 to indicate major rewrite)

---

## Resources

### Python Libraries
- **Core**: click/typer, pydantic, rich
- **LaTeX**: subprocess (pdflatex), jinja2 (templates)
- **Graphics**: matplotlib, graphviz, pillow
- **Testing**: pytest, pytest-cov, hypothesis
- **Quality**: mypy, ruff, bandit, safety
- **AI**: anthropic, openai, langchain (optional)

### Security Resources

**Local Reference Documents** (see `docs/security/` directory):
- **OWASP LLM Top 10** - `LLMAll_en-US_FINAL.pdf` (comprehensive guide, 8.4 MB)
- **OWASP Top 10 for Agentic Applications 2026** - Latest guidance for AI agents
- **OWASP GenAI COMPASS RunBook** - Operational security procedures
- **OWASP GenAI Solutions Reference Guide** - Technical implementations and code examples
- **MCP Server Security CheatSheet** - Third-party integration best practices

**Online Resources**:
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/)
- [Bandit SAST Tool](https://bandit.readthedocs.io/)
- [Safety Dependency Scanner](https://github.com/pyupio/safety)

📖 See `docs/security/README.md` for detailed descriptions and relevance ratings for each reference document.

### Development Resources
- [Python Packaging Guide](https://packaging.python.org/)
- [Poetry Documentation](https://python-poetry.org/)
- [pytest Documentation](https://docs.pytest.org/)
- [mypy Type Checking](https://mypy.readthedocs.io/)

---

**Last Updated**: 2026-01-12
**Next Review**: After Phase 0 completion
**Next Milestone**: Milestone 0 - Architecture Ready
