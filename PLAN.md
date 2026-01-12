# SCIgen Modernization Plan

**Created**: 2026-01-12
**Status**: Planning Phase
**Branch**: `claude/init-archived-project-2Tefy`

## Overview

This document provides an atomic, actionable plan to modernize the SCIgen codebase from its 2005 state to a modern, maintainable project suitable for AI enhancement.

---

## Phase 0: Foundation & Assessment

**Goal**: Understand current state and establish baseline functionality

### 0.1 Environment Setup
- [ ] Test if Perl scripts run on modern systems
- [ ] Document which dependencies are already installed
- [ ] Identify missing dependencies (LaTeX, gnuplot, etc.)
- [ ] Test basic paper generation: `./make-latex.pl --author "Test" --seed 1 --savedir test_output`
- [ ] Document what works and what breaks

### 0.2 Basic Project Hygiene
- [ ] Create `.gitignore` for LaTeX artifacts (`*.aux`, `*.log`, `*.dvi`, `*.bbl`, `*.blg`, `*.ps`)
- [ ] Add temp directories to `.gitignore` (`/tmp/`, `/test_output/`)
- [ ] Create `CONTRIBUTING.md` with development guidelines
- [ ] Update `README.md` with actual setup and usage instructions
- [ ] Add version/changelog tracking (create `CHANGELOG.md`)

### 0.3 Documentation
- [x] Create `CLAUDE.md` with comprehensive project documentation
- [x] Create `PLAN.md` (this file)
- [ ] Add inline code comments to core functions in `scigen.pm`
- [ ] Document grammar rule syntax in dedicated `GRAMMAR.md`
- [ ] Create architecture diagram showing component relationships

---

## Phase 1: Dependencies & Portability

**Goal**: Make the project run reliably on modern systems

### 1.1 Dependency Management
- [ ] Create `cpanfile` listing all Perl module dependencies
- [ ] Create `system-dependencies.txt` listing external tools (LaTeX, gnuplot, etc.)
- [ ] Write `install-deps.sh` script for Ubuntu/Debian
- [ ] Write `install-deps-mac.sh` script for macOS (brew-based)
- [ ] Test dependency installation on clean Ubuntu container
- [ ] Test dependency installation on clean macOS system

### 1.2 Path & Filesystem Fixes
- [ ] Replace hardcoded `/tmp` with `File::Temp->newdir()` in `make-latex.pl`
- [ ] Replace hardcoded `/tmp` with `File::Temp->newdir()` in `make-graph.pl`
- [ ] Replace hardcoded `/tmp` with `File::Temp->newdir()` in `make-diagram.pl`
- [ ] Replace hardcoded `/tmp` with `File::Temp->newdir()` in `make-talk-figure.pl`
- [ ] Make scripts work from any directory (use `FindBin` for relative paths)
- [ ] Update file path handling to use `File::Spec` for cross-platform compatibility
- [ ] Test running scripts from different working directories

### 1.3 External Tool Updates
- [ ] Replace `gv` viewer with modern alternative (`evince`, `okular`, or skip viewer)
- [ ] Replace `acroread` with modern PDF viewer or remove dependency
- [ ] Update `ps2epsi` calls or replace with modern alternatives
- [ ] Add fallback options for missing optional tools (graceful degradation)
- [ ] Add tool detection script that checks for all dependencies
- [ ] Update LaTeX compilation to support both old and new toolchains

### 1.4 LaTeX Template Updates
- [ ] Research latest IEEEtran.cls version and compatibility
- [ ] Update `IEEEtran.cls` to latest version (or keep for compatibility)
- [ ] Test paper generation with updated templates
- [ ] Verify PDF output quality and formatting
- [ ] Document any breaking changes in template updates

---

## Phase 2: Code Quality & Security

**Goal**: Modernize code practices and fix security issues

### 2.1 Perl Modernization
- [ ] Add `use strict;` and `use warnings;` to all scripts
- [ ] Fix all strict/warnings violations in `make-latex.pl`
- [ ] Fix all strict/warnings violations in `make-graph.pl`
- [ ] Fix all strict/warnings violations in `make-diagram.pl`
- [ ] Fix all strict/warnings violations in `make-talk-figure.pl`
- [ ] Fix all strict/warnings violations in `scigen.pm`
- [ ] Fix all strict/warnings violations in `scigend`
- [ ] Replace `our` with proper scoping where appropriate
- [ ] Add POD (Perl documentation) to `scigen.pm`
- [ ] Add POD to main scripts

### 2.2 Security Hardening
- [ ] Audit all `system()` calls in `make-latex.pl`
- [ ] Replace unsafe `system()` calls with list form or IPC::Run
- [ ] Sanitize file paths passed to external commands
- [ ] Review `eval()` usage for mathematical expressions
- [ ] Add input validation for command-line arguments
- [ ] Add input validation for author names (prevent shell injection)
- [ ] Add input validation for system names
- [ ] Add input validation for seed values
- [ ] Run security linter (Perl::Critic) and fix issues
- [ ] Document security considerations in SECURITY.md

### 2.3 Error Handling
- [ ] Add proper error handling to file operations in `scigen.pm`
- [ ] Add meaningful error messages for missing dependencies
- [ ] Add error handling for LaTeX compilation failures
- [ ] Add error handling for graph generation failures
- [ ] Add error handling for diagram generation failures
- [ ] Replace `die` with more user-friendly error messages
- [ ] Add `--debug` flag for verbose error output
- [ ] Create error code reference in documentation

### 2.4 Code Refactoring
- [ ] Extract magic numbers to named constants
- [ ] Break down long functions in `make-latex.pl` (> 50 lines)
- [ ] Separate concerns in `scigen.pm` (parsing vs. expansion vs. formatting)
- [ ] Remove dead code and commented-out sections
- [ ] Standardize variable naming conventions
- [ ] Add type hints/documentation for function parameters
- [ ] Create utility module for common operations

### 2.5 Command-Line Interface
- [ ] Add `--help` flag to `make-latex.pl`
- [ ] Add `--version` flag to all scripts
- [ ] Improve `--help` output with examples
- [ ] Add `--quiet` flag to suppress output
- [ ] Add `--verbose` flag for detailed progress
- [ ] Standardize exit codes (0 = success, 1 = error, 2 = usage)
- [ ] Add `--dry-run` flag to preview without generating

---

## Phase 3: Testing & Validation

**Goal**: Ensure reliability and prevent regressions

### 3.1 Testing Framework
- [ ] Install Test::More and Test::Simple
- [ ] Create `t/` directory for tests
- [ ] Write test for grammar file parsing (`t/01-parse-rules.t`)
- [ ] Write test for basic rule expansion (`t/02-expand-rules.t`)
- [ ] Write test for paper generation with seed (`t/03-generate-paper.t`)
- [ ] Write test for graph generation (`t/04-generate-graph.t`)
- [ ] Write test for diagram generation (`t/05-generate-diagram.t`)
- [ ] Write test for LaTeX compilation (`t/06-latex-compile.t`)
- [ ] Add regression tests for known bugs
- [ ] Create test helper utilities

### 3.2 Bug Fixes
- [ ] Fix bug #1: Author names appearing multiple times in single reference
- [ ] Write test to verify bug #1 fix
- [ ] Fix bug #2: Author name mismatches between citations and text
- [ ] Write test to verify bug #2 fix
- [ ] Test all known edge cases from TODO and IDEAS files
- [ ] Fix any newly discovered bugs
- [ ] Document all bug fixes in CHANGELOG.md

### 3.3 CI/CD Setup
- [ ] Create `.github/workflows/test.yml` for GitHub Actions
- [ ] Add CI job: Perl syntax check (perl -c)
- [ ] Add CI job: Perl::Critic linting
- [ ] Add CI job: Run test suite
- [ ] Add CI job: Test paper generation
- [ ] Add CI job: Test on Ubuntu latest
- [ ] Add CI job: Test on macOS latest
- [ ] Add status badge to README.md

### 3.4 Validation
- [ ] Generate 100 papers with different seeds, verify no crashes
- [ ] Verify LaTeX output compiles cleanly
- [ ] Verify generated PDFs are readable
- [ ] Compare output with original SCIgen (spot check)
- [ ] Test all command-line flag combinations
- [ ] Test edge cases (empty author, very long names, special characters)
- [ ] Performance benchmark (time to generate 10 papers)

---

## Phase 4: Containerization & Distribution

**Goal**: Make deployment easy and consistent

### 4.1 Docker Support
- [ ] Create `Dockerfile` with all dependencies
- [ ] Base image: Ubuntu LTS with Perl, LaTeX, gnuplot
- [ ] Test build: `docker build -t scigen .`
- [ ] Test run: `docker run scigen --author "Test"`
- [ ] Optimize image size (multi-stage build if needed)
- [ ] Create `docker-compose.yml` for easy usage
- [ ] Add volume mounts for output directory
- [ ] Document Docker usage in README.md
- [ ] Push image to Docker Hub (optional)

### 4.2 Alternative Packaging
- [ ] Create installation script (`install.sh`)
- [ ] Consider creating `.deb` package (Debian/Ubuntu)
- [ ] Consider creating Homebrew formula (macOS)
- [ ] Consider creating snap package
- [ ] Test installation on clean systems
- [ ] Add uninstall script
- [ ] Document installation methods in README.md

### 4.3 Release Process
- [ ] Set up semantic versioning (MAJOR.MINOR.PATCH)
- [ ] Tag first modernized release as `v2.0.0`
- [ ] Create GitHub release with binaries/archives
- [ ] Update CHANGELOG.md for each release
- [ ] Create release checklist document
- [ ] Automate release process with GitHub Actions

---

## Phase 5: Enhanced Features

**Goal**: Add modern functionality while preserving core behavior

### 5.1 Output Format Options
- [ ] Add `--format pdf` flag (default to PDF instead of PS)
- [ ] Add `--format html` flag (LaTeX → HTML conversion)
- [ ] Add `--format docx` flag (via pandoc)
- [ ] Add `--format markdown` flag (simplified output)
- [ ] Test all output formats
- [ ] Document format options in help text

### 5.2 Configuration System
- [ ] Create `~/.scigenrc` support for default options
- [ ] Support project-level `.scigen.conf` files
- [ ] Add `--config` flag to specify custom config
- [ ] Document configuration file format
- [ ] Add example configuration files

### 5.3 Extended Grammar Support
- [ ] Create `rules/` directory for grammar organization
- [ ] Move grammar files to `rules/` directory
- [ ] Update scripts to look in `rules/` directory
- [ ] Create grammar file for biology papers (`rules/bio.in`)
- [ ] Create grammar file for physics papers (`rules/physics.in`)
- [ ] Add `--field` flag to select domain (cs, bio, physics)
- [ ] Test cross-domain generation

### 5.4 Citation Style Support
- [ ] Research APA citation format requirements
- [ ] Create `APA.bst` BibTeX style
- [ ] Research MLA citation format requirements
- [ ] Create `MLA.bst` BibTeX style
- [ ] Add `--cite-style` flag (ieee, apa, mla, chicago)
- [ ] Test paper generation with different citation styles
- [ ] Document citation style options

### 5.5 Additional Content Types
- [ ] Add theorem generation (based on IDEAS file)
- [ ] Add lemma generation
- [ ] Add proof generation (structured gibberish)
- [ ] Add table generation with random data
- [ ] Add equation generation (beyond current math)
- [ ] Add algorithm pseudocode generation
- [ ] Add experimental data tables
- [ ] Test integration with main paper

### 5.6 Web Interface (Optional)
- [ ] Design simple web UI mockup
- [ ] Choose web framework (Flask/FastAPI for Python, Express for Node.js)
- [ ] Create API wrapper around Perl scripts
- [ ] Build form for author names, seed, options
- [ ] Add real-time generation progress indicator
- [ ] Add download button for generated PDF
- [ ] Add gallery of example papers
- [ ] Deploy web interface (Heroku, Vercel, or Docker)
- [ ] Document API endpoints

---

## Phase 6: AI Integration

**Goal**: Enhance SCIgen with modern AI capabilities

### 6.1 LLM-Enhanced Content Generation
- [ ] Research appropriate AI models (GPT-4, Claude, Llama)
- [ ] Design prompt templates for paper sections
- [ ] Add `--ai-mode` flag for LLM-enhanced generation
- [ ] Generate more coherent abstracts using LLM
- [ ] Generate more coherent introductions using LLM
- [ ] Keep methodology/results as gibberish (original behavior)
- [ ] Add coherence control slider (0% = pure gibberish, 100% = full LLM)
- [ ] Test output quality at different coherence levels
- [ ] Compare generation speed (classic vs AI-enhanced)
- [ ] Document AI integration in README.md

### 6.2 Grammar Rule Generation
- [ ] Design LLM prompts to generate grammar rules
- [ ] Generate additional CS vocabulary using AI
- [ ] Expand system_names.in with AI-generated terms
- [ ] Add rules for emerging tech (blockchain, ML, quantum)
- [ ] Validate generated rules don't break expansion engine
- [ ] Create tool to auto-expand grammar files with AI

### 6.3 Style Transfer
- [ ] Collect corpus of papers from specific authors/journals
- [ ] Extract stylistic features (sentence length, vocabulary, structure)
- [ ] Add `--mimic` flag to imitate style
- [ ] Test mimicry of famous CS authors (Knuth, Dijkstra, etc.)
- [ ] Add disclaimer about ethical use
- [ ] Document style transfer capabilities

### 6.4 SCIgen Detection
- [ ] Build classifier to detect SCIgen-generated papers
- [ ] Train on corpus of real papers vs SCIgen papers
- [ ] Create `scigen-detect.pl` script
- [ ] Add API endpoint for detection service
- [ ] Test accuracy on historical accepted SCIgen papers
- [ ] Document detection methodology
- [ ] Consider releasing as separate tool

### 6.5 Interactive Customization
- [ ] Add interactive mode (`--interactive` flag)
- [ ] Prompt user for topic preferences
- [ ] Let user guide section content
- [ ] Preview sections before final generation
- [ ] Allow regeneration of specific sections
- [ ] Save custom preferences for future use

### 6.6 Paper Review Generation
- [ ] Generate fake peer reviews for generated papers
- [ ] Include typical reviewer comments (clarity, novelty, etc.)
- [ ] Add randomized accept/reject decisions
- [ ] Generate author responses to reviews
- [ ] Create full review cycle simulation
- [ ] Document review generation usage

---

## Milestones & Success Criteria

### Milestone 1: "It Runs" (Phase 0-1)
**Goal**: SCIgen works reliably on modern systems
- All scripts execute without errors
- Dependencies documented and installable
- Basic paper generation works end-to-end
- Docker image available

### Milestone 2: "It's Solid" (Phase 2-3)
**Goal**: Code is clean, secure, and tested
- No security vulnerabilities
- Test coverage > 80%
- All known bugs fixed
- CI/CD passing on all platforms

### Milestone 3: "It's Modern" (Phase 4-5)
**Goal**: Easy to use and extend
- Multiple output formats supported
- Additional content types available
- Web interface deployed (optional)
- Documentation complete

### Milestone 4: "It's Enhanced" (Phase 6)
**Goal**: AI-powered features working
- LLM integration functional
- Detection system operational
- Style transfer demonstrated
- Interactive mode available

---

## Notes & Decisions

### What to Preserve
- Original grammar files (for historical authenticity)
- Core expansion algorithm (it works well)
- Seed-based reproducibility (for testing/comparison)
- Command-line interface (add to it, don't replace)

### What to Change
- File paths and temp file handling
- Dependency management
- Error handling and messaging
- Security practices
- Documentation

### What to Add
- Testing framework
- Docker support
- Web interface (optional)
- AI enhancements (optional)
- Multiple output formats

### Open Questions
- [ ] Should we maintain backward compatibility with old grammar files?
- [ ] Should we create a plugin system for custom content generators?
- [ ] Should we support multiple languages (i18n)?
- [ ] Should we create a desktop GUI (Electron/Qt)?
- [ ] What's the best way to distribute pre-built binaries?

---

## Timeline Estimates

**Note**: These are rough estimates for a single developer working part-time.

- **Phase 0-1**: 1-2 weeks (Foundation & Portability)
- **Phase 2-3**: 2-3 weeks (Code Quality & Testing)
- **Phase 4**: 1 week (Containerization)
- **Phase 5**: 2-3 weeks (Enhanced Features)
- **Phase 6**: 3-4 weeks (AI Integration)

**Total**: 9-13 weeks for full modernization

---

## Getting Started

### First Sprint (Immediate Tasks)
1. Complete Phase 0.1 (Environment Setup)
2. Create `.gitignore`
3. Update README.md
4. Test basic generation
5. Document what works/breaks

### Quick Wins (Low-hanging fruit)
- Add `--help` flag
- Create `.gitignore`
- Fix `/tmp` hardcoding
- Add basic error messages
- Create Docker container

---

**Last Updated**: 2026-01-12
**Next Review**: After Phase 1 completion
