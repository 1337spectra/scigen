# SCIgen Python Rewrite Plan

**Created**: 2026-01-12
**Updated**: 2026-01-12
**Status**: Planning Phase → Python Rewrite
**Branch**: `claude/init-archived-project-2Tefy`
**Approach**: **Security-First Python Rewrite with OWASP Compliance**

## Overview

This document provides an atomic, actionable plan to **rewrite SCIgen from Perl to Python** with security and AI as first-class requirements from day one. Rather than modernizing 21-year-old Perl code, we're building a sustainable, secure, **AI-native** foundation suitable for long-term maintenance and enhancement.

**Key Architectural Decision**: LLM capabilities are built into the infrastructure from Phase 0, not bolted on in Phase 7. This ensures:
- No costly refactoring later
- Security designed for AI from the start (prompt injection prevention, output sanitization)
- Modular architecture (classic, AI, hybrid modes)
- Graceful degradation (falls back to classic if LLM unavailable)

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
- [ ] Design module structure (`scigen/`, `scigen/core/`, `scigen/generators/`, `scigen/llm/`)
- [ ] Define Pydantic models for configuration
- [ ] Design Pydantic models for input validation
- [ ] Choose CLI framework (Click vs Typer)
- [ ] Choose plotting library (matplotlib vs plotly)
- [ ] **Design LLM abstraction layer** (provider-agnostic interface)
- [ ] **Design content generation strategy pattern** (classic vs AI vs hybrid)
- [ ] **Plan secure prompt template system** (prevent injection from day one)
- [ ] Design plugin architecture for extensibility
- [ ] Create architecture diagram (modules, data flow, LLM integration points)

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

### 0.6 LLM Infrastructure Planning (AI-Native Design)
- [ ] **Design content generator interface** (abstract base for classic/AI implementations)
- [ ] **Plan LLM provider abstraction** (support anthropic, openai, local models)
- [ ] **Design secure prompt template system** (structured, injection-resistant)
- [ ] **Plan API key management** (environment, secrets, rotation)
- [ ] **Design token usage tracking** (limits, cost monitoring, alerts)
- [ ] **Plan content coherence system** (0-100% slider between gibberish and coherent)
- [ ] **Design caching strategy** (cache LLM responses for determinism/cost)
- [ ] **Plan fallback mechanisms** (graceful degradation if LLM unavailable)
- [ ] Document LLM security threat model
- [ ] Create LLM integration test strategy

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

### 1.5 LLM Abstraction Layer (Foundation)
- [ ] Create `ContentGenerator` protocol/ABC (classic vs AI implementations)
- [ ] Create `ClassicGenerator` (rule-based, current SCIgen behavior)
- [ ] Create `LLMGenerator` interface (to be implemented incrementally)
- [ ] Create `HybridGenerator` (mix classic structure with AI content)
- [ ] Implement `LLMProvider` ABC (anthropic, openai, local)
- [ ] **Security**: Design secure prompt template system (Jinja2-based)
- [ ] **Security**: Input sanitization for LLM prompts
- [ ] Implement basic prompt validation
- [ ] Add optional dependencies (anthropic, openai as extras)
- [ ] Unit tests for generator selection logic

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

### 2.5 Command-Line Interface (AI-Ready from Day One)
- [ ] Implement CLI with Click/Typer
- [ ] Add `--author` flag (multiple authors supported)
- [ ] Add `--seed` flag (reproducibility)
- [ ] Add `--file` flag (output path)
- [ ] Add `--savedir` flag (save source)
- [ ] Add `--sysname` flag (custom system name)
- [ ] **Add `--mode` flag** (classic, ai, hybrid) - defaults to classic
- [ ] **Add `--coherence` flag** (0-100, controls AI vs gibberish ratio)
- [ ] **Add `--llm-provider` flag** (anthropic, openai, local)
- [ ] **Add `--ai-sections` flag** (which sections to AI-enhance: abstract, intro, all)
- [ ] Add `--help` with examples (include AI mode examples)
- [ ] Add `--version` flag
- [ ] **Security**: Validate all CLI inputs (Pydantic)
- [ ] Rich output (progress bars, colors, LLM usage stats)
- [ ] Comprehensive help text with AI feature documentation

### 2.6 LLM Integration (Basic Implementation)
- [ ] Implement `AnthropicProvider` (Claude API integration)
- [ ] Implement `OpenAIProvider` (GPT API integration)
- [ ] **Security**: Environment-based API key loading (.env)
- [ ] **Security**: API key validation (never log keys)
- [ ] Implement basic prompt templates for abstracts
- [ ] Implement basic prompt templates for introductions
- [ ] **Security**: Output sanitization for LLM responses (LaTeX escaping)
- [ ] Implement token usage tracking
- [ ] Implement timeout protection (30s default)
- [ ] Add graceful fallback to classic mode if LLM fails
- [ ] Unit tests for LLM providers (mocked)
- [ ] Integration tests (optional, requires API keys)

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

### 4.4 Security Testing (Including LLM Security)
- [ ] **Security**: Prompt injection test suite (critical!)
  - [ ] Direct injection attempts in author names
  - [ ] Indirect injection via system names
  - [ ] Jailbreak attempts in custom prompts
  - [ ] Template escape attempts
- [ ] **Security**: LaTeX injection attempts
- [ ] **Security**: File path traversal attempts
- [ ] **Security**: Command injection attempts
- [ ] **Security**: DoS attempts (large files, deep recursion, token bombs)
- [ ] **Security**: API key leakage tests (logs, outputs, errors)
- [ ] **Security**: Dependency vulnerability scanning
- [ ] **Security**: SAST with Bandit
- [ ] **Security**: DAST (Dynamic testing)
- [ ] Penetration testing checklist
- [ ] LLM-specific security audit (OWASP LLM Top 10)

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

## Phase 7: Advanced AI Features

**Goal**: Add sophisticated LLM features (foundation already built in Phase 0-2)

**Architecture Note**: Unlike traditional "Phase 7 = add AI" approaches, we're building LLM capabilities into the infrastructure from day one:
- **Phase 0**: LLM architecture planning, provider abstraction design
- **Phase 1**: Content generator interfaces, LLM abstraction layer
- **Phase 2**: Basic LLM integration (abstracts, intros, `--mode ai` flag)
- **Phase 4**: LLM security testing (prompt injection, etc.)
- **Phase 7**: Advanced features (style transfer, detection, agentic behavior)

This avoids costly refactoring and ensures security is baked in from the start.

### 7.1 Enhanced LLM Content Generation
- [ ] Implement LLM-enhanced methodology sections
- [ ] Implement LLM-enhanced evaluation sections
- [ ] Implement LLM-enhanced related work
- [ ] Add `--full-ai` flag (entire paper AI-generated)
- [ ] Fine-tune prompt templates based on feedback
- [ ] Implement multi-shot prompting (examples in context)
- [ ] Add temperature control for creativity
- [ ] Implement reasoning traces (chain-of-thought)
- [ ] Add section-by-section regeneration
- [ ] Quality evaluation metrics (coherence scoring)

### 7.2 Advanced Prompt Engineering
- [ ] Implement few-shot learning prompts
- [ ] Add domain-specific prompt templates (CS, bio, physics)
- [ ] Implement prompt chaining (multi-step generation)
- [ ] Add meta-prompting (LLM generates prompts)
- [ ] Implement prompt optimization based on outputs
- [ ] **Security**: Advanced injection detection (anomaly detection)
- [ ] **Security**: Prompt firewall (reject suspicious patterns)
- [ ] A/B testing framework for prompt effectiveness

### 7.3 Local LLM Support
- [ ] Integrate llama-cpp-python for local models
- [ ] Support Ollama integration
- [ ] Add `--local-model` flag
- [ ] Implement model downloading and caching
- [ ] Optimize for local GPU (CUDA, Metal)
- [ ] Compare quality: local vs API models
- [ ] Privacy-focused mode (all local, no API calls)

### 7.4 Grammar Rule Generation with AI
- [ ] Design prompts for vocabulary generation
- [ ] Tool: `scigen-expand-grammar --ai` for auto-expansion
- [ ] Expand system_names.in with LLM (emerging tech terms)
- [ ] Generate domain-specific vocabularies (quantum, bio, ML)
- [ ] LLM-generated grammar rules (validated before inclusion)
- [ ] **Security**: Human review of AI-generated rules
- [ ] Versioned grammar files (track AI vs human)
- [ ] Quality metrics for generated rules

### 7.5 Style Transfer & Mimicry
- [ ] Collect corpus of academic papers (various authors)
- [ ] Extract stylistic features (RAG-based retrieval)
- [ ] Design style transfer prompts
- [ ] Add `--mimic` flag (author names, journal styles)
- [ ] Test mimicry (Knuth, Dijkstra, Lamport styles)
- [ ] Implement style intensity control
- [ ] **Security**: Validate style inputs
- [ ] Ethical use disclaimer
- [ ] Document limitations and intended use

### 7.6 SCIgen Detection & Adversarial Generation
- [ ] Build detection classifier (real vs SCIgen papers)
- [ ] Train on corpus (scikit-learn or transformer)
- [ ] Create `scigen-detect` CLI command
- [ ] Add detection API endpoint
- [ ] **Adversarial mode**: Generate papers that evade detection
- [ ] **Security**: Rate limit detection API
- [ ] Test on historical accepted SCIgen papers
- [ ] Publish accuracy metrics
- [ ] Consider separate detection package

### 7.7 Interactive & Agentic Mode
- [ ] Add `--interactive` CLI flag
- [ ] Multi-turn conversation (refine paper iteratively)
- [ ] Preview sections with LLM summaries
- [ ] Allow section-by-section regeneration
- [ ] Topic guidance ("focus on X, avoid Y")
- [ ] Save conversation history and preferences
- [ ] **Agentic features**: Multi-step reasoning (OWASP Agentic Top 10)
- [ ] **Security**: Validate all interactive inputs (per OWASP guidance)
- [ ] Rich terminal UI (prompt toolkit)

### 7.8 Peer Review & Meta-Generation
- [ ] Generate fake peer reviews (LLM-enhanced)
- [ ] Include typical reviewer comments (clarity, novelty, rigor)
- [ ] Randomized accept/reject decisions with justification
- [ ] Generate author responses (rebuttal generation)
- [ ] Simulate full review cycle (3 rounds)
- [ ] Add `--with-reviews` flag
- [ ] Meta-commentary generation (editor notes, area chair)
- [ ] Conference acceptance letter generation

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

### Milestone 0: "Architecture Ready - AI-Native Foundation" (Phase 0)
**Criteria**:
- Python project structure created
- Development tooling configured (pytest, mypy, ruff)
- Security infrastructure set up (Safety, Bandit)
- Architecture documented
- **LLM abstraction layer designed**
- **Content generation strategy pattern defined**
- **Prompt security architecture documented**

### Milestone 1: "Grammar Engine + LLM Foundation" (Phase 1)
**Criteria**:
- Grammar parser complete
- Rule expansion matches Perl outputs
- **Content generator interfaces implemented**
- **LLM provider abstraction layer working**
- Unit tests passing (>90% coverage)
- Security tests passing (injection, fuzzing, prompt injection)

### Milestone 2: "Feature Parity + Basic AI" (Phase 2-3)
**Criteria**:
- Generate complete papers matching Perl (classic mode)
- **AI mode functional (abstracts, intros)**
- **`--mode`, `--coherence`, `--llm-provider` flags working**
- LaTeX compilation working
- Figures and diagrams generated
- All original bugs fixed
- Integration tests passing (both classic and AI modes)
- **LLM security tests passing**

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

### Milestone 5: "Advanced AI Features" (Phase 7)
**Criteria**:
- Full-paper AI generation working
- Local LLM support functional
- Style transfer operational
- Detection classifier trained and tested
- Interactive/agentic mode available
- Peer review generation working
- Grammar expansion with AI functional
- All OWASP LLM Top 10 mitigations validated
- OWASP Agentic Applications Top 10 compliance

---

## Timeline Estimates

**Note**: Estimates for single developer, part-time (10-15 hrs/week)

- **Phase 0**: 1-2 weeks (Architecture & Planning, including LLM design)
- **Phase 1**: 3-4 weeks (Core Grammar Engine + LLM Abstraction Layer)
- **Phase 2**: 3-4 weeks (Paper Generation + Basic AI Integration)
- **Phase 3**: 1-2 weeks (Graphics & Diagrams)
- **Phase 4**: 1-2 weeks (Testing & QA, including LLM security)
- **Phase 5**: 1 week (Packaging & Distribution)
- **Phase 6**: 2-3 weeks (Enhanced Features)
- **Phase 7**: 3-4 weeks (Advanced AI Features)

**Total**: 14-20 weeks (3.5-5 months part-time)

**Note**: Timeline is similar to original plan, but we get basic AI features by Milestone 2 instead of waiting until Milestone 5. Advanced AI features come in Phase 7.

---

## Getting Started

### Immediate Next Steps (Phase 0)

1. **Study Perl Code** (3-4 hours)
   - Read `scigen.pm` thoroughly
   - Trace rule expansion logic
   - Document all features
   - Identify LLM integration points

2. **Design LLM Architecture** (2-3 hours)
   - Sketch content generator interface
   - Design LLM provider abstraction
   - Plan prompt template system
   - Document security requirements

3. **Set Up Python Project** (1-2 hours)
   ```bash
   poetry new scigen-python
   cd scigen-python
   poetry add click pydantic pytest mypy ruff
   poetry add --group dev pytest-cov bandit safety
   poetry add --optional anthropic openai  # AI extras
   ```

4. **Create Project Structure** (1 hour)
   ```
   scigen-python/
   ├── src/scigen/
   │   ├── __init__.py
   │   ├── core/          # Grammar engine
   │   ├── generators/    # Paper, graph, diagram generators
   │   ├── llm/          # LLM providers, prompts, security
   │   ├── cli/          # Command-line interface
   │   └── utils/        # Utilities
   ├── tests/
   │   ├── unit/
   │   ├── integration/
   │   └── security/     # Prompt injection tests
   ├── docs/
   ├── grammar/          # Copy .in files
   └── pyproject.toml
   ```

5. **Configure Tools** (1 hour)
   - Set up pytest configuration
   - Configure mypy (strict mode)
   - Configure ruff
   - Set up pre-commit hooks
   - Configure python-dotenv for API keys

6. **Start Phase 1** (ongoing)
   - Begin grammar parser implementation
   - Design content generator interfaces
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
