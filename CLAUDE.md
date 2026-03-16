# Audit Kit

**Educational Codebase Auditing for Claude Code**

Audit Kit provides a focused set of auditing agents that teach you how to assess your codebase systematically. Rather than just running checks and reporting results, these agents explain what they're looking for, why it matters, and how you can perform these audits yourself.

---

## Philosophy: Learn to Audit

**Audit Kit is designed for learning, not just doing.**

Each auditor agent follows three principles:

1. **Explain Before Acting** - Before scanning, the agent explains what it's looking for and why
2. **Show Your Work** - Findings include the methodology used, not just results
3. **Teach the Patterns** - Each audit builds your understanding of what "good" looks like

This approach serves two purposes:
- **Consulting Foundation**: Demonstrates expertise while delivering value
- **User Empowerment**: Transfers knowledge so users can audit independently

---

## The Core Auditors

Audit Kit includes four universal auditors that apply to any technology stack:

| Auditor | Domain | What It Teaches |
|---------|--------|-----------------|
| **security-auditor** | Security posture | Secret detection, config hardening, access patterns |
| **dependency-auditor** | Supply chain health | Vulnerability assessment, maintenance status, update strategies |
| **codebase-auditor** | Project health | Structure patterns, code organization, documentation completeness |
| **standards-auditor** | Convention compliance | Consistency checking, pattern enforcement, drift detection |

### When to Use Each

**security-auditor**
- Before deploying to production
- After adding new integrations or APIs
- During security reviews or compliance checks
- When onboarding to an unfamiliar codebase

**dependency-auditor**
- Before major releases
- When dependency update alerts appear
- During quarterly maintenance windows
- When evaluating project health

**codebase-auditor**
- When starting on a new project
- Before major refactoring efforts
- During code review process improvements
- When assessing technical debt

**standards-auditor**
- After establishing team conventions
- When onboarding new team members
- During consistency reviews across projects
- Before creating templates or boilerplate

---

## Usage Patterns

### Quick Security Check

```
"Run a security audit on this project"
"Check for exposed secrets"
"Assess our authentication configuration"
```

### Dependency Health Assessment

```
"Audit our dependencies"
"Check for vulnerable packages"
"Assess the maintenance status of our dependencies"
```

### Codebase Structure Review

```
"Analyze this codebase structure"
"Assess our documentation completeness"
"Check our project organization"
```

### Standards Compliance

```
"Check this project against standard patterns"
"Find inconsistencies in our codebase"
"Assess our configuration drift"
```

---

## Audit Output Philosophy

Every audit produces:

1. **Methodology Section** - What was checked and how
2. **Findings** - What was discovered, with severity ratings
3. **Evidence** - Specific files and patterns that support each finding
4. **Learning Points** - What this teaches about good practices
5. **Recommendations** - Actionable next steps with trade-offs explained

### Severity Levels

| Level | Meaning | Example |
|-------|---------|---------|
| **Critical** | Immediate action needed | Exposed API keys in source |
| **High** | Address this week | Known vulnerable dependency |
| **Medium** | Plan to address | Missing input validation |
| **Low** | Consider improving | Inconsistent naming patterns |
| **Info** | Good to know | Documentation coverage |

---

## Educational Features

### Pattern Recognition

Auditors don't just flag problems - they explain the pattern:

```
Finding: API key hardcoded in source file

Pattern Explanation:
API keys should never be committed to source control because:
1. Version history retains secrets even after removal
2. Anyone with repository access gains API access
3. Key rotation becomes complex when embedded in code

Better Pattern:
Store secrets in environment variables or secret management
systems. Reference them via process.env or equivalent.
```

### Methodology Transparency

Each audit explains its approach:

```
Security Audit Methodology:

1. Secret Detection
   - Scanned for patterns: API keys, tokens, passwords
   - Checked locations: source files, configs, environment files
   - Verified: .gitignore coverage for sensitive files

2. Configuration Review
   - Checked: Debug modes, verbose logging, permissive CORS
   - Verified: Production-appropriate settings

3. Access Pattern Analysis
   - Reviewed: Authentication configuration
   - Checked: Authorization boundaries
```

---

## Integration with Consulting

Audit Kit is designed as a foundation for consulting engagements:

### Discovery Phase

Use auditors to quickly understand a new codebase:
1. Run codebase-auditor for structure overview
2. Run security-auditor to identify immediate risks
3. Run dependency-auditor to assess technical debt
4. Run standards-auditor to understand conventions

### Recommendations Phase

Audit reports provide:
- Evidence-based findings (not opinions)
- Severity-prioritized action items
- Trade-off explanations for each recommendation
- Clear methodology that can be repeated

### Knowledge Transfer

The educational approach ensures:
- Clients understand the "why" behind recommendations
- Teams can perform follow-up audits independently
- Patterns learned transfer to future projects

---

## Reference Documentation

The `reference/` directory contains:

- **audit-patterns.md** - Common audit patterns across domains
- **severity-guide.md** - How to assess and prioritize findings
- **remediation-patterns.md** - Common fixes for common findings

---

## Boundaries

### Auditors DO:

- Scan and analyze codebases
- Explain what they're looking for and why
- Provide evidence-based findings
- Teach patterns and best practices
- Suggest remediation with trade-offs

### Auditors DON'T:

- Modify any files (strictly read-only)
- Auto-fix problems without approval
- Access external services or APIs
- Make assumptions without verification
- Expose sensitive information in reports

---

## Getting Started

1. **Deploy to your project**: Copy agents to `.claude/agents/`
2. **Start with security**: Run security-auditor first on any new project
3. **Build understanding**: Read the methodology sections to learn the patterns
4. **Expand gradually**: Add other auditors as needed

---

## Room to Grow

This is the core set. Future auditors could include:

- **performance-auditor** - Bundle sizes, load times, optimization opportunities
- **accessibility-auditor** - A11y compliance and inclusive design
- **api-auditor** - API design patterns and documentation
- **infrastructure-auditor** - Cloud configuration and deployment patterns

The patterns established by the core four provide a foundation for domain-specific extensions.

---

*Audit Kit: Because understanding your codebase is the first step to improving it.*
