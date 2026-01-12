# CLAUDE.md - SCIgen Project Documentation

## Project Overview

**SCIgen** (SCIentific paper GENerator) is an automatic computer science research paper generator created in 2005 by three MIT CSAIL graduate students: Jeremy Stribling, Dan Aguayo, and Max Krohn.

### What It Does

SCIgen generates random, nonsensical computer science research papers that appear legitimate, complete with:
- Realistic academic structure (abstract, introduction, methodology, evaluation, related work, conclusion)
- Graphs and figures (scatter plots, bar charts, line graphs, CDFs)
- Network diagrams using Graphviz
- Citations and bibliographies
- Proper LaTeX formatting using IEEE style

### Historical Significance

One SCIgen-generated paper titled "Rooter: A Methodology for the Typical Unification of Access Points and Redundancy" was accepted without review to the 9th World Multi-Conference on Systemics, Cybernetics and Informatics (WMSCI) in 2005. This exposed serious problems with predatory conferences. Later, IEEE and Springer removed 120+ SCIgen-generated papers that had been unknowingly published.

## Project Structure

```
/home/user/scigen/
├── Core Engine
│   ├── scigen.pm              - Context-free grammar expansion engine
│   ├── Autoformat.pm          - Text formatting utilities
│   └── Reform.pm              - Text reformatting support
│
├── Main Scripts
│   ├── make-latex.pl          - Main paper generation script
│   ├── make-graph.pl          - Graph/chart generator
│   ├── make-diagram.pl        - Network diagram generator
│   ├── make-talk-figure.pl    - Presentation figure generator
│   ├── scigen.pl              - Generic grammar expander
│   ├── scigend                - Daemon for system name generation
│   └── test-scigend.pl        - Test script for daemon
│
├── Grammar Rules (.in files)
│   ├── scirules.in            - Main paper generation rules (3,777 lines)
│   ├── system_names.in        - System name vocabulary (79,515 lines!)
│   ├── talkrules.in           - Presentation slide rules
│   ├── svg_figures.in         - SVG figure generation rules
│   ├── graphviz.in            - Graph diagram rules
│   └── functions.in           - Mathematical function rules
│
├── LaTeX Support
│   ├── IEEEtran.cls           - IEEE LaTeX document class (2002 version)
│   └── IEEE.bst               - IEEE BibTeX style
│
└── Documentation
    ├── README.md              - Minimal readme (needs expansion)
    ├── LICENSE / COPYING      - GNU GPL v2
    ├── TODO                   - Known bugs
    └── IDEAS                  - Future enhancement ideas
```

## Technology Stack

### Original Implementation (2005)
- **Perl 5** (version 5.005+, from 1998)
- Modified CPAN modules (Autoformat.pm, Reform.pm)
- Context-free grammar expansion engine
- External tools: LaTeX, gnuplot, Graphviz

### Python Rewrite (2026+)

**Decision**: After evaluating sustainability, we've chosen to **rewrite SCIgen in Python** rather than modernize the existing Perl codebase.

**Rationale**:
- **Long-term maintainability**: Python has a larger, more active community
- **AI ecosystem**: Superior libraries for LLM integration (OpenAI SDK, Anthropic SDK, LangChain)
- **Modern tooling**: Better testing (pytest), packaging (Poetry), type hints
- **Developer experience**: More accessible for contributions and future maintenance
- **Sustainability**: Python is a top-tier language unlikely to decline

**Core Stack**:
- **Python 3.11+** - Modern Python with performance improvements
- **Poetry** - Dependency management and packaging
- **Pydantic** - Data validation and settings management
- **Click/Typer** - Modern CLI framework
- **pytest** - Testing framework with excellent coverage tools

**External Dependencies**:

**LaTeX Toolchain** (required):
- pdflatex/lualatex - Direct PDF generation (skip DVI/PS)
- bibtex - Bibliography management
- Modern LaTeX distribution (TeX Live 2023+)

**Graphics Tools**:
- **matplotlib** - Native Python plotting (replaces gnuplot)
- **Graphviz** (via Python bindings) - Network diagram generation
- **Pillow** - Image manipulation (replaces ImageMagick)

**AI/LLM Integration** (optional):
- anthropic - Claude API for content enhancement
- openai - GPT integration
- langchain - LLM orchestration framework

## How to Run

### Generate a Paper

Basic usage:
```bash
./make-latex.pl --author "John Doe" --author "Jane Smith"
```

With seed for reproducibility:
```bash
./make-latex.pl --author "Alice" --seed 12345
```

Save to specific file:
```bash
./make-latex.pl --author "Bob" --file output.ps
```

Generate presentation slides:
```bash
./make-latex.pl --author "Charlie" --talk --file slides.pdf
```

Save source without compiling:
```bash
./make-latex.pl --author "Dave" --savedir /path/to/output
```

Package as tarball:
```bash
./make-latex.pl --author "Eve" --tar paper.tar.gz
```

Custom system name:
```bash
./make-latex.pl --author "Frank" --sysname "QuantumDB"
```

### Generate Individual Components

Graph only:
```bash
./make-graph.pl --file mygraph.eps --seed 42
```

Diagram only:
```bash
./make-diagram.pl --sys "MySystem" --file diagram.eps --seed 99
```

Test grammar rules:
```bash
./scigen.pl --filename scirules.in --start SCI_TITLE
```

### Run as Daemon

```bash
./scigend &
./test-scigend.pl
```

## Core Architecture

### Grammar Expansion Engine (`scigen.pm`)

The heart of SCIgen is a context-free grammar (CFG) expansion engine with these features:

- **`read_rules()`** - Parses grammar rule files
- **`expand()`** - Recursively expands grammar rules
- **`generate()`** - Main generation entry point
- **`pretty_print()`** - Formats output (capitalization, LaTeX cleanup)

### Grammar Rule Features

- **Weighted rules**: `RULE+N` syntax for higher probability selection
- **Non-duplicate rules**: `RULE!` syntax to prevent duplicates
- **File inclusion**: `.include` directive
- **Sequential counters**: `RULE+` for incrementing values
- **Random selection**: `RULE#` for random choice

### Generation Pipeline

1. Parse grammar rules from `.in` files
2. Expand starting rule recursively
3. Generate required figures/graphs/diagrams
4. Insert figures into LaTeX template
5. Generate bibliography entries
6. Compile: LaTeX → BibTeX → LaTeX → DVI → PS/PDF

## Current State (2026 Assessment)

### What Still Works ✓
- Core grammar expansion engine is solid
- Paper structure generation
- Graph generation with gnuplot
- LaTeX compilation pipeline
- Seed-based reproducibility

### What's Outdated (21 Years Old) ⚠️

**Perl Issues**:
- Uses Perl 5.005+ (from 1998)
- Modified CPAN modules instead of using standard library
- No modern Perl practices (no Moose/Moo, inconsistent strict/warnings)

**Path/Filesystem Issues**:
- Hardcoded `/tmp` directory usage
- Relative path assumptions (must run from project root)
- Uses process ID for temp file naming (potential collisions)

**External Tool Dependencies**:
- `gv` (ghostview) is obsolete → use `evince`, `okular`, or `zathura`
- `acroread` (Adobe Reader) is obsolete
- `ps2epsi` may not be available on modern systems
- LaTeX toolchain has evolved (lualatex, xelatex available)

**Security Issues**:
- Uses `system()` calls with potential injection risks
- Uses `eval()` on generated mathematical expressions
- No input sanitization

**LaTeX Issues**:
- Uses old IEEEtran.cls from 2002 (v1.6b)
- Modern IEEE templates have changed significantly
- Paper format may look dated

**Missing Modern Features**:
- No containerization (Docker)
- No package manager support (CPAN, cpanfile)
- No CI/CD integration
- No web interface
- No testing framework
- No version management

## Known Issues

From `TODO` file:
1. Author names appearing multiple times in single reference
2. Author name mismatches between citations and text

## Security Requirements (OWASP)

As we rewrite SCIgen and add AI capabilities, **security is a first-class requirement**. We will follow OWASP guidelines throughout development.

### OWASP Top 10 (Web Application Security)

**A01: Broken Access Control**
- Implement proper authentication for web interface (if added)
- Rate limiting on API endpoints
- Validate all file paths to prevent directory traversal

**A02: Cryptographic Failures**
- Store API keys in environment variables, never in code
- Use secrets management (e.g., python-dotenv, environment configs)
- Encrypt sensitive data at rest and in transit

**A03: Injection**
- **Prompt Injection Prevention** (critical for AI features)
- Sanitize all user inputs (author names, system names, custom prompts)
- Use parameterized LaTeX generation (avoid string concatenation)
- Validate grammar rule inputs
- Never use `eval()` or `exec()` on user input

**A04: Insecure Design**
- Principle of least privilege (minimal file system access)
- Fail securely (default deny, explicit allow)
- Defense in depth (multiple validation layers)

**A05: Security Misconfiguration**
- Secure default configurations
- Minimal dependencies (reduce attack surface)
- Keep dependencies updated (Dependabot/Renovate)
- Disable debug mode in production

**A06: Vulnerable and Outdated Components**
- Regular dependency scanning (Safety, pip-audit)
- Automated dependency updates
- Pin dependency versions with Poetry
- Monitor CVE databases for vulnerabilities

**A07: Identification and Authentication Failures**
- Strong authentication for admin features
- Session management for web interface
- MFA support if handling sensitive operations

**A08: Software and Data Integrity Failures**
- Sign releases with GPG
- Verify dependencies with checksums
- Use trusted package sources only
- Implement integrity checks for grammar files

**A09: Security Logging and Monitoring**
- Log all security-relevant events
- Monitor API usage (rate limiting, abuse detection)
- Alert on suspicious patterns
- Privacy-compliant logging (no PII in logs)

**A10: Server-Side Request Forgery (SSRF)**
- Validate all URLs if fetching external resources
- Allowlist approach for external services
- No arbitrary external requests based on user input

### OWASP Top 10 for LLM Applications (AI-Specific)

**LLM01: Prompt Injection**
- **Critical**: User inputs must never directly modify system prompts
- Use structured prompts with clear boundaries
- Validate and sanitize all user-provided content
- Implement prompt templates with safe parameter substitution
- Example: `"Generate a paper with author: {sanitized_author}"` not `f"{user_input}"`

**LLM02: Insecure Output Handling**
- Sanitize LLM outputs before rendering in LaTeX
- Escape special characters (%, $, \, {, })
- Validate LLM-generated content structure
- Rate limit to prevent abuse

**LLM03: Training Data Poisoning**
- Not applicable (we're using external LLM APIs, not training models)
- Document which models/versions are supported

**LLM04: Model Denial of Service**
- Implement token limits on API calls
- Request throttling and rate limiting
- Timeout long-running LLM operations
- Cost controls on API usage

**LLM05: Supply Chain Vulnerabilities**
- Use official LLM SDKs only (anthropic, openai)
- Verify package integrity
- Pin SDK versions
- Monitor for SDK vulnerabilities

**LLM06: Sensitive Information Disclosure**
- Never send API keys to LLM
- Sanitize outputs for potential credential leakage
- Don't log full LLM requests/responses (may contain keys)
- Implement data retention policies

**LLM07: Insecure Plugin Design**
- If building plugin system, sandbox all plugins
- Minimal permissions for plugins
- Code review all community plugins

**LLM08: Excessive Agency**
- LLM should only generate text, not execute code
- No file system write access for LLM outputs (human review first)
- Explicit user confirmation for destructive actions
- Clear boundaries on LLM capabilities

**LLM09: Overreliance**
- Document that LLM-generated content is unverified
- Human review for any production use
- Disclaimers about paper quality
- Not for academic submission

**LLM10: Model Theft**
- Not applicable (using API, not hosting models)
- Protect API keys to prevent unauthorized usage

### Security Implementation Checklist

- [ ] Input validation framework (Pydantic models)
- [ ] Output sanitization for LaTeX special characters
- [ ] Secrets management (python-dotenv)
- [ ] Dependency scanning (Safety/pip-audit in CI)
- [ ] Rate limiting (for API and web endpoints)
- [ ] Logging framework with security events
- [ ] Error handling that doesn't leak sensitive info
- [ ] Security testing suite
- [ ] SECURITY.md with vulnerability reporting process
- [ ] Regular security audits
- [ ] Prompt injection testing suite
- [ ] API key rotation procedures

### Security Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)

## Python Rewrite Roadmap

See **PLAN.md** for detailed, atomic task breakdown. High-level phases:

### Phase 0: Architecture & Design (Foundation)
- [ ] Study Perl grammar engine thoroughly
- [ ] Design Python architecture (module structure, data models)
- [ ] Document grammar rule syntax formally
- [ ] Create Python project structure with Poetry
- [ ] Design Pydantic models for security (input validation)
- [ ] Set up testing infrastructure (pytest)
- [ ] Set up security scanning (Safety, Bandit)

### Phase 1: Core Grammar Engine (Critical Path)
- [ ] Port grammar file parser from Perl to Python
- [ ] Implement rule expansion algorithm
- [ ] Support weighted rules (`RULE+N`)
- [ ] Support non-duplicate rules (`RULE!`)
- [ ] Support sequential counters (`RULE+`)
- [ ] Support random selection (`RULE#`)
- [ ] Implement pretty_print/formatting logic
- [ ] **Security**: Input validation for all grammar parsing
- [ ] Unit tests with 100% coverage against Perl reference outputs

### Phase 2: Paper Generation (Feature Parity)
- [ ] Port main paper generation script
- [ ] LaTeX template system (with safe variable substitution)
- [ ] Bibliography generation (BibTeX integration)
- [ ] **Security**: Sanitize all LaTeX special characters
- [ ] Support all original CLI flags (--author, --seed, --file, etc.)
- [ ] Modern CLI with Click/Typer (help text, validation)
- [ ] Error handling (friendly messages, proper exit codes)
- [ ] Integration tests matching Perl outputs

### Phase 3: Graphics & Diagrams
- [ ] Port graph generation to matplotlib
- [ ] Port diagram generation (Graphviz Python bindings)
- [ ] Mathematical function generation
- [ ] SVG/PNG figure generation
- [ ] Figure embedding in LaTeX
- [ ] Visual regression tests (compare outputs to Perl version)

### Phase 4: Testing & Quality
- [ ] Fix known bugs (duplicate authors, name mismatches)
- [ ] Regression test suite (100+ generated papers)
- [ ] Performance benchmarks
- [ ] **Security**: Prompt injection test suite
- [ ] **Security**: Fuzzing for input validation
- [ ] Code coverage > 90%
- [ ] Type checking with mypy (strict mode)
- [ ] Linting with ruff
- [ ] Documentation (docstrings, type hints)

### Phase 5: Packaging & Distribution
- [ ] Docker container (multi-stage build)
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] **Security**: Dependency scanning in CI
- [ ] **Security**: SAST (Static Application Security Testing)
- [ ] PyPI package distribution
- [ ] Installation scripts
- [ ] Comprehensive README
- [ ] User documentation
- [ ] Contributor guide

### Phase 6: Enhanced Features
- [ ] Multiple output formats (PDF, HTML, Markdown)
- [ ] Configuration system (~/.scigenrc)
- [ ] Additional academic fields (biology, physics)
- [ ] Multiple citation styles (APA, MLA, Chicago)
- [ ] Web interface (FastAPI + simple UI)
- [ ] **Security**: Authentication/rate limiting for web UI
- [ ] REST API for programmatic access
- [ ] **Security**: API authentication with tokens

### Phase 7: AI Integration
- [ ] LLM-enhanced abstract generation (Claude/GPT)
- [ ] **Security**: Prompt injection prevention framework
- [ ] Coherence control (0-100% slider)
- [ ] Grammar rule generation with AI
- [ ] Style transfer (mimic specific authors)
- [ ] SCIgen detection classifier
- [ ] Interactive paper customization
- [ ] Peer review generation
- [ ] **Security**: Token limits and cost controls
- [ ] **Security**: Output sanitization for LLM responses

### Security Throughout All Phases
- Input validation with Pydantic (every user input)
- Output sanitization (LaTeX, HTML escaping)
- Secrets management (environment variables, never hardcoded)
- Dependency updates (automated with Dependabot)
- Security audit logs
- Regular penetration testing
- OWASP compliance checklist validation

## Development Tips

### Testing Changes
Run basic generation test:
```bash
./make-latex.pl --author "Test Author" --seed 1 --savedir test_output
```

Check grammar expansion:
```bash
./scigen.pl --filename scirules.in --start SCI_ABSTRACT
```

### Grammar Rule Development
Grammar files use simple substitution syntax:
```
RULE_NAME
    option 1
    option 2
    option 3 can reference OTHER_RULE
```

Weighted rules (3x more likely):
```
RULE_NAME
    common option+3
    rare option
```

Non-duplicate rules:
```
AUTHOR_NAME!
    Alice
    Bob
```

### Common Pitfalls
- Scripts must be run from repository root
- LaTeX compilation requires all tools in PATH
- Grammar rules are case-sensitive
- Circular rule references will cause infinite loops
- Large grammar files can cause memory issues

## Resources

### Original Project
- Website (archived): https://pdos.csail.mit.edu/archive/scigen/
- Original authors: Jeremy Stribling, Dan Aguayo, Max Krohn (MIT CSAIL)

### Related Projects
- **scigen.js**: JavaScript port with web interface
- Various online generators based on SCIgen

### Academic Papers
- Look for publications about SCIgen that discuss its impact on academic publishing

## License

GNU General Public License v2 (GPL-2.0)

See `LICENSE` and `COPYING` files for full text.

---

**Last Updated**: 2026-01-12
**Repository**: Fork of archived MIT CSAIL SCIgen project
**Status**: Python rewrite planned - security-first approach with OWASP compliance
**Next Phase**: Phase 0 - Architecture & Design
