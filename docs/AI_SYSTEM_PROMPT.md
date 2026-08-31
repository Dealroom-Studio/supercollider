# SuperCollider AI System Prompt
## Workflow Production & Infrastructure Guidelines

**Version:** 1.0  
**Last Updated:** 2026-08-31  
**Scope:** AI-assisted development, infrastructure, and product workflows

---

## Executive Summary

This document establishes system-level guidelines for AI infrastructure and product development workflows in SuperCollider. It serves as permanent documentation for AI agents, developers, and infrastructure systems collaborating on audio synthesis, DSP algorithms, and IDE features.

---

## 1. Core Project Context

### Project Identity
**SuperCollider** is a real-time audio server, programming language, and IDE for sound synthesis and algorithmic composition.

**Technology Stack:**
- **Primary Language:** C++ (92%)
- **Audio Server:** scsynth (real-time DSP), supernova (parallel processing)
- **Language:** sclang (interpreted, controls servers)
- **IDE:** scide (integrated help, editing environment)
- **Package Manager:** Quarks (third-party plugin support)
- **Dependencies:** Qt 6.2+, Boost, JACK audio

### Platform Support (Guaranteed)
- Windows 10, 11 (MSVC 2019, 2022)
- macOS 13–15 (Xcode 14–16)
- Linux: Debian ≥11, Ubuntu 22.04–24.04, Fedora 36–37, Arch
- Raspberry Pi, Bela (embedded audio)
- Compiler: gcc ≥9, clang ≥11

### Repository Structure
- **Parent Upstream:** [supercollider/supercollider](https://github.com/supercollider/supercollider)
- **Default Branch:** `develop`
- **License:** GNU General Public License v3.0

---

## 2. AI Infrastructure Guidelines

### 2.1 Code Generation Standards

#### C++ Audio DSP Guidelines
- **Standard:** C++17 minimum
- **Real-time Safety:** Avoid dynamic memory allocation in audio-rate operations
- **Lock-Free Primitives:** Use atomic operations and lock-free data structures for server-client communication
- **Plugin API:** Follow UGen C++ API patterns; document parameter ranges and audio-rate/control-rate validity
- **Performance:** Optimize for multi-core processors; use SIMD where applicable for buffer processing

#### Language-Specific Conventions

| Language | Usage | Standard | Conventions |
|----------|-------|----------|-------------|
| C++ | Audio server, core DSP, IDE | C++17 | camelCase, templates for generic DSP, RAII patterns |
| SuperCollider | Language runtime, music notation | sclang spec | Class definitions, message passing, FunctionDefs |
| CMake | Build system | 3.13+ | Modular configuration, platform-specific targets |
| Objective-C++ | macOS integration | Modern | Retain/release semantics, Cocoa bindings |

### 2.2 Development Workflow

#### Branch Strategy
- **develop:** Integration branch for in-progress features
- **feature/\*:** New features and enhancements
- **bugfix/\*:** Bug fixes targeting develop
- **hotfix/\*:** Production patches
- All PRs require code review before merge

#### Testing Standards
- **Unit Tests:** C++ server components (Boost.Test framework)
- **Integration Tests:** sclang interpreter, scsynth communication
- **Platform CI:** GitHub Actions for Windows, macOS, Linux
- **Audio Correctness:** Unit generator output validation, buffer integrity checks

#### Documentation Conventions
- **Code Comments:** Explain *why* decisions were made, not *what* the code does
- **Public APIs:** Doxygen-compatible headers with parameter descriptions
- **Audio Components:** Document block size constraints, sample rate dependencies, threading safety
- **UGen Help Files:** SCDoc format with examples, argument ranges, and CPU cost estimates

### 2.3 API and SDK Development

#### Plugin Architecture
- **C API:** Stable, versioned interface for external plugins
- **C++ UGen API:** Template-based synthesis primitive framework
- **Quarks Integration:** Package metadata, dependency resolution, version compatibility
- **Backward Compatibility:** Maintain across minor versions; document breaking changes

#### AI-Assisted Implementation
- When generating plugin code:
  1. Validate against existing UGen patterns in `external_libraries/` and `server/plugins/`
  2. Include safety guards: sample-rate checks, buffer bounds, denormal handling
  3. Document audio rate vs. control rate behavior
  4. Provide Quarks metadata template
- When generating sclang classes:
  1. Follow SC naming conventions (camelCase, class names start with capital)
  2. Include help file templates (SCDoc format)
  3. Test with audio server connection

---

## 3. Product Development Standards

### 3.1 Feature Development

#### Requirements Specification
- **Issue Template:** Use GitHub issue templates for feature requests
- **Specification:** Link to issue in commit messages and PRs
- **Scope Definition:** Clearly delineate MVP vs. future enhancements
- **Platform Impact:** Assess implications for Windows, macOS, Linux, embedded systems

#### Audio-First Principles
- Features must not introduce latency or audio dropouts
- Real-time safety takes precedence over convenience
- User-facing changes to DSP require audible documentation (examples, demos)
- Performance regression testing on target hardware (Raspberry Pi, multi-core systems)

### 3.2 Quality Assurance

#### Code Review Checklist
- ✓ Compliance with platform support requirements
- ✓ No breaking changes to stable APIs without deprecation
- ✓ Thread-safety verified for multi-threaded components
- ✓ Memory management: no leaks, proper RAII, atomic cleanup
- ✓ Cross-platform testing results (CI passing)
- ✓ Documentation: comments, help files, release notes updated
- ✓ Performance impact quantified (if applicable)

#### Regression Prevention
- Maintain upstream `supercollider/supercollider` sync cadence (monthly review)
- Run full test suite on supported platforms before release
- Test with external plugins (Quarks ecosystem)
- Audio fidelity testing: bit-depth preservation, DSP accuracy

### 3.3 Release and Deployment

#### Version Management
- **Semantic Versioning:** MAJOR.MINOR.PATCH
- **Stability Markers:** alpha, beta, rc, release
- **Deprecation Cycle:** 2-3 minor versions warning before removal
- **Release Channels:** Stable (documentation), Development (nightly CI builds)

#### Deployment Checklist
- [ ] All platform CI builds passing
- [ ] Release notes drafted with feature list and API changes
- [ ] Upgrade guide for deprecated features
- [ ] Help documentation updated for new features
- [ ] GitHub Releases drafted with binary links
- [ ] Quarks registry updated (new packages, versions)

---

## 4. Permission & Authorization Framework

### 4.1 Repository Access

| Role | Permissions | Responsibilities |
|------|-------------|------------------|
| **Maintainer** | Admin, approve/merge PRs | Releases, strategic direction, breaking decisions |
| **Developer** | Push to feature branches, create PRs | Implementation, testing, documentation |
| **Contributor** | Fork, create PR from fork | Bug fixes, minor features, community support |
| **AI Agent** | Read + suggest changes | Code analysis, test generation, documentation drafting |

### 4.2 CI/CD Authorization
- **GitHub Actions:** Workflows run on:
  - Every PR merge to `develop`
  - Tagged releases (automatic deployment)
  - Scheduled nightly builds
- **Artifact Storage:** Release binaries hosted on downloads page; Quarks registry synced via CI

### 4.3 External Integration
- **Quarks Registry:** AI agents may assist in metadata validation, not auto-publish
- **Upstream Sync:** Pull requests to `supercollider/supercollider` require maintainer approval
- **Third-Party Libraries:** License compliance review before inclusion (Boost, Qt, JACK)

---

## 5. AI Agent Behavior & Constraints

### 5.1 Appropriate AI Tasks
✅ **Recommended for AI Assistance:**
- Code generation for new UGens following existing patterns
- SCDoc help file templating
- Test case generation (unit + integration)
- Cross-platform build troubleshooting
- Documentation drafting and proofreading
- Static analysis and code cleanup suggestions
- Performance profiling recommendations

### 5.2 Tasks Requiring Human Review
⚠️ **Mandatory Human Oversight:**
- Breaking API changes (requires architecture review)
- Audio DSP algorithm changes affecting output
- Security-related patches (permissions, file I/O)
- Upstream `supercollider/supercollider` contributions
- Feature prioritization and scope decisions
- Release management and version bumping

### 5.3 Constraints & Boundaries
- **Real-Time Guarantees:** AI suggestions must preserve or improve latency profile
- **Backward Compatibility:** Maintain API stability unless explicitly deprecated
- **Audio Quality:** All DSP changes require validation against reference implementations
- **Community Standards:** Respect code of conduct and contributor guidelines
- **License Compliance:** All generated code inherits GPL v3.0 license

---

## 6. Specification & Standards References

### 6.1 Audio & DSP Standards
- **JACK Audio Connection Kit:** [jackaudio.org](https://jackaudio.org/) – Inter-app audio routing
- **Audio Formats:** WAV (PCM), AIFF, FLAC metadata support
- **DSP References:**
  - Vercoe, B.L. et al. *The Csound Book* (MIT Press) – Signal processing fundamentals
  - Smith, J.O. *Physical Audio Signal Processing* (CCRMA) – Real-time DSP techniques

### 6.2 Development Standards
- **C++ Standard:** ISO/IEC 14882:2017 (C++17)
- **Qt Framework:** [qt.io](https://www.qt.io/) – UI/multiplatform framework
- **Boost Libraries:** [boost.org](https://www.boost.org/) – Memory management, threading, utilities
- **CMake Build System:** [cmake.org](https://cmake.org/) – Cross-platform compilation

### 6.3 Software Specification Formats
- **SCDoc:** SuperCollider documentation markup (help files)
- **Doxygen:** C++ API documentation
- **JSON:** Quarks metadata and configuration
- **YAML:** CI/CD workflow definition (GitHub Actions)

### 6.4 Compliance & Infrastructure
- **GitHub Workflows:** Actions for CI/CD automation
- **Platform Testing:**
  - Windows: MSVC compiler, AppVeyor CI
  - macOS: Xcode, Apple Silicon support
  - Linux: gcc/clang, glibc compatibility
- **Accessibility:** IDE UI must support screen reader integration, keyboard navigation

---

## 7. Common Workflows & Recipes

### 7.1 Adding a New UGen (Audio Unit Generator)

**Steps for AI-Assisted Implementation:**

1. **Research existing UGen:**
   - Locate similar UGen in `server/plugins/` or `external_libraries/`
   - Review C++ template pattern and registration macro

2. **Generate C++ skeleton:**
   - Use `DEFINE_CTOR`, `CALC`, and `DTOR` macros
   - Implement `next_k()` (control rate) and `next_a()` (audio rate) variants
   - Add parameter validation and denormal protection

3. **Create sclang class wrapper:**
   - Define in appropriate Quarks/class hierarchy
   - Specify `ar` (audio rate) and `kr` (control rate) constructors
   - Document with SCDoc template

4. **Write tests:**
   - Unit tests for parameter bounds and edge cases
   - Audio output validation (frequency response, amplitude)
   - Load test with multi-instance scenarios

5. **Document:**
   - SCDoc help file with examples
   - API comments (Doxygen) for C++ class
   - Release notes entry

### 7.2 Platform Porting (e.g., New Embedded System)

**Workflow:**

1. Verify platform prerequisites (compiler, JACK support)
2. Create platform-specific CMake configuration
3. Generate platform-specific README (template: `README_PLATFORM.md`)
4. Run CI tests; document known limitations
5. Update [Platform Support Wiki](https://github.com/supercollider/supercollider/wiki/Platform-Support)

### 7.3 Contributing to Upstream `supercollider/supercollider`

**Authorization & Workflow:**

1. Ensure change aligns with upstream roadmap
2. Create feature branch in this fork
3. Generate PR with detailed description, issue references
4. Obtain maintainer approval for upstream sync
5. Submit PR to `supercollider/supercollider` with Dealroom-Studio attribution

---

## 8. Troubleshooting & Edge Cases

### Common Scenarios

| Scenario | AI Action | Human Review |
|----------|-----------|--------------|
| Build fails on platform X | Analyze logs, suggest CMake/dependency fixes | Maintainer verifies on native hardware |
| Audio UGen produces distortion | Review DSP math, suggest denormal guards | DSP expert validates against reference |
| API conflict with upstream | Identify breaking change, propose deprecation path | Architecture decision required |
| Quarks dependency resolution fails | Trace metadata, suggest version constraints | Maintainer resolves conflicts |

---

## 9. Escalation & Maintenance

### Issues Requiring Maintainer Attention
- Security vulnerabilities
- Performance regressions >5% on benchmark suite
- Upstream sync conflicts
- License compliance concerns
- Breaking API changes
- User accessibility issues

### Quarterly Review Checklist
- [ ] Upstream `supercollider/supercollider` merge status
- [ ] Dependency updates (Qt, Boost, JACK)
- [ ] Platform support verification (nightly CI results)
- [ ] Deprecated API removal schedule
- [ ] Quarks ecosystem health (package updates, security)
- [ ] Community feedback synthesis (forum, issues)

---

## 10. Contact & Documentation

### Official Resources
- **Main Repository:** [supercollider/supercollider](https://github.com/supercollider/supercollider)
- **Fork:** [Dealroom-Studio/supercollider](https://github.com/Dealroom-Studio/supercollider)
- **Documentation:** [dev.docs.supercollider.online](https://dev.docs.supercollider.online)
- **Forum:** [scsynth.org](https://scsynth.org/)
- **Code of Conduct:** [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md)

### Contributing
- Read [CONTRIBUTING.md](../CONTRIBUTING.md) (if present, or reference upstream Wiki)
- Review [Code of Conduct](../CODE_OF_CONDUCT.md)
- Join [Slack community](https://join.slack.com/t/scsynth/shared_invite/zt-ezoyz15j-SVM7JVul94pxtDiUDRnd0w)

---

**Document Status:** Active  
**Next Review:** Q1 2027  
**Maintainers:** Dealroom-Studio team
