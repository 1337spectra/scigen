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

### Core Language
- **Perl 5** (version 5.005+, from 1998)

### External Dependencies

**LaTeX Toolchain** (required):
- latex, bibtex - Document typesetting
- dvips - DVI to PostScript conversion
- ps2pdf - PostScript to PDF conversion (Ghostscript)
- ps2epsi - PostScript to EPS conversion

**Graphics Tools**:
- gnuplot - Required for graph generation
- Graphviz (neato/dot) - Optional, for network diagrams
- Inkscape - Optional, for SVG to PNG conversion
- ImageMagick (convert) - Optional, for image format conversion

**Viewers** (optional):
- gv (ghostview) - PostScript viewer (outdated)
- acroread - PDF viewer (outdated)

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

## Modernization Roadmap

### Phase 1: Basic Modernization
- [ ] Add proper dependency management (cpanfile or requirements file)
- [ ] Create Dockerfile for easy setup
- [ ] Update LaTeX templates to modern versions
- [ ] Replace obsolete viewer dependencies
- [ ] Add proper logging instead of print statements
- [ ] Fix hardcoded /tmp paths (use File::Temp)
- [ ] Add comprehensive README with setup instructions
- [ ] Add testing framework (Test::More)
- [ ] Fix known bugs from TODO

### Phase 2: Code Quality
- [ ] Update to modern Perl practices
- [ ] Add input sanitization for security
- [ ] Replace system() calls with safer alternatives
- [ ] Add proper error handling
- [ ] Add command-line help (--help flag)
- [ ] Modularize code better
- [ ] Add code documentation (POD)

### Phase 3: New Features
- [ ] Web interface (consider Node.js/Python with existing Perl core)
- [ ] Support multiple academic fields (beyond CS)
- [ ] Support multiple citation styles (APA, MLA, Chicago, etc.)
- [ ] API for integration with other tools
- [ ] Generate additional content types:
  - Theorems and proofs
  - Tables and equations
  - Experimental data
- [ ] Multiple language support via grammar files

### Phase 4: AI Integration Ideas
- [ ] Use LLMs to generate more coherent (but still nonsensical) content
- [ ] AI-powered grammar rule generation
- [ ] Style transfer (mimic specific authors/journals)
- [ ] Automated detection of SCIgen-generated papers
- [ ] Interactive paper customization
- [ ] Generate reviews/responses to papers

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
**Status**: Ready for modernization
