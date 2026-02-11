# Code Review Expert

A comprehensive code review skill for AI agents. Performs structured reviews with a senior engineer lens, covering architecture, security, performance, and code quality.

## About this fork

This repository is a **fork** of the original [code-review-expert](https://github.com/sanyuan0704/code-review-expert) project with the following enhancements:

- **Language-specific checklists** – Added dedicated checklists for platform/language detection and review:
  - **Android** (`references/android-checklist.md`) – Kotlin/Java: memory, lifecycle, ANR, coroutines
  - **iOS** (`references/ios-checklist.md`) – Swift: ARC, main-thread safety, Swift idioms
  - **Frontend** (`references/fe-checklist.md`) – Vue/TS: reactivity, component lifecycle, render performance

Install from this fork if you want these extra checklists; otherwise use the original for the base skill.


## Features

- **SOLID Principles** - Detect SRP, OCP, LSP, ISP, DIP violations
- **Security Scan** - XSS, injection, SSRF, race conditions, auth gaps, secrets leakage
- **Performance** - N+1 queries, CPU hotspots, missing cache, memory issues
- **Error Handling** - Swallowed exceptions, async errors, missing boundaries
- **Boundary Conditions** - Null handling, empty collections, off-by-one, numeric limits
- **Removal Planning** - Identify dead code with safe deletion plans

## Usage

After installation, simply run:

```
/code-review-expert
```

The skill will automatically review your current git changes.

## Workflow

1. **Preflight** - Scope changes via `git diff`
2. **SOLID + Architecture** - Check design principles
3. **Removal Candidates** - Find dead/unused code
4. **Security Scan** - Vulnerability detection
5. **Code Quality** - Error handling, performance, boundaries
6. **Output** - Findings by severity (P0-P3)
7. **Confirmation** - Ask user before implementing fixes

## Severity Levels

| Level | Name | Action |
|-------|------|--------|
| P0 | Critical | Must block merge |
| P1 | High | Should fix before merge |
| P2 | Medium | Fix or create follow-up |
| P3 | Low | Optional improvement |

## Structure

```
code-review-expert/
├── SKILL.md                 # Main skill definition
├── agents/
│   └── agent.yaml           # Agent interface config
└── references/
    ├── solid-checklist.md       # SOLID smell prompts
    ├── security-checklist.md   # Security & reliability
    ├── code-quality-checklist.md # Error, perf, boundaries
    ├── android-checklist.md     # Android (Kotlin/Java) specific
    ├── ios-checklist.md        # iOS (Swift) specific
    ├── fe-checklist.md         # Frontend (Vue/TS) specific
    └── removal-plan.md         # Deletion planning template
```

## References

Each checklist provides detailed prompts and anti-patterns:

- **solid-checklist.md** - SOLID violations + common code smells
- **security-checklist.md** - OWASP risks, race conditions, crypto, supply chain
- **code-quality-checklist.md** - Error handling, caching, N+1, null safety
- **android-checklist.md** - Memory/lifecycle, ANR, Kotlin practices (this fork)
- **ios-checklist.md** - ARC, main-thread safety, Swift idioms (this fork)
- **fe-checklist.md** - Reactivity, component lifecycle, render performance (this fork)
- **removal-plan.md** - Safe vs deferred deletion with rollback plans

## License

MIT
