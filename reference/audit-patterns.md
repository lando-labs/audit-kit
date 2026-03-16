# Audit Patterns Reference

Common patterns used across all Audit Kit auditors.

---

## Universal Audit Structure

Every audit follows a three-phase structure:

### Phase 1: Orientation
- Explain what will be checked
- Educate on why each check matters
- Set expectations for findings

### Phase 2: Assessment
- Execute checks with transparency
- Document methodology as you go
- Gather evidence for findings

### Phase 3: Reporting
- Present findings with context
- Include learning points
- Provide actionable recommendations

---

## Finding Structure

Every finding should include:

```markdown
### [SEVERITY] Finding Title

**Location**: Where the issue was found
**Category**: Which audit domain this falls under

**What I Found**:
Description of the finding with evidence

**Why This Matters** (Learning Point):
Explanation of the risk or cost

**Evidence**:
Specific examples supporting the finding

**Recommendation**:
1. Immediate action
2. Follow-up action
3. Preventive measure

**Trade-offs**:
What to consider when implementing the fix
```

---

## Severity Assessment

### Critical
- Immediate action required
- Security exposure, data loss risk, system instability
- Examples: Exposed secrets, critical vulnerabilities, broken authentication

### High
- Address within days
- Significant risk but not immediately exploitable
- Examples: Known CVEs, missing authentication, major dependency issues

### Medium
- Plan to address within weeks
- Moderate risk or significant friction
- Examples: Configuration issues, inconsistent patterns, documentation gaps

### Low
- Address when convenient
- Minor issues or improvement opportunities
- Examples: Style inconsistencies, minor documentation updates

### Info
- Good to know
- No action required, but worth noting
- Examples: Positive practices, neutral observations

---

## Evidence Collection

### Source Code Evidence
```markdown
**File**: `src/services/api.ts`
**Line**: 45
**Content**: [Relevant code snippet or pattern]
```

### Configuration Evidence
```markdown
**File**: `.eslintrc.json`
**Setting**: `"no-unused-vars": "off"`
**Concern**: [Why this setting matters]
```

### Pattern Evidence
```markdown
**Pattern A**: [Description] - Found in X files
**Pattern B**: [Description] - Found in Y files
**Dominant**: Pattern A (X/(X+Y) = Z%)
```

### Metric Evidence
```markdown
| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| Test coverage | 45% | 70% | Below |
| Dependency age | 18mo | 12mo | Exceeded |
```

---

## Common Audit Categories

### Security
- Secret exposure
- Configuration security
- Dependency vulnerabilities
- Authentication/authorization
- Data handling

### Dependencies
- Known vulnerabilities
- Maintenance status
- Version currency
- Dependency depth
- License compliance

### Codebase
- Project structure
- Documentation completeness
- Code organization
- Testing presence
- Development infrastructure

### Standards
- Naming conventions
- File organization
- Code patterns
- Configuration consistency
- Documentation style

---

## Tool Usage Patterns

### File Discovery
```bash
# Find all files of a type
Glob: **/*.ts

# Find configuration files
Glob: **/.eslintrc* | **/eslint.config.*

# Find test files
Glob: **/*.test.* | **/*.spec.* | **/__tests__/**
```

### Pattern Search
```bash
# Find security patterns
Grep: password\s*=|api_key\s*=|secret\s*=

# Find TODO comments
Grep: TODO|FIXME|HACK|XXX

# Find console statements
Grep: console\.(log|error|warn)
```

### Content Analysis
```bash
# Read specific file
Read: package.json

# Read with context
Read: .gitignore
```

### System Analysis
```bash
# File counts
Bash: find ./src -name "*.ts" | wc -l

# Directory structure
Bash: ls -la ./src

# Package audit
Bash: npm audit --json
```

---

## Report Templates

### Executive Summary Template
```markdown
## Executive Summary

This [audit type] of [project name] identified [X critical, Y high,
Z medium, W low] findings across [list domains].

**Key Strengths**:
- [Positive finding 1]
- [Positive finding 2]

**Key Concerns**:
- [Critical/High finding 1]
- [Critical/High finding 2]

**Recommended Priority**:
1. [First action]
2. [Second action]
```

### Health Dashboard Template
```markdown
## Health Dashboard

| Dimension | Status | Score | Notes |
|-----------|--------|-------|-------|
| [Dim 1] | [OK/Concern/Critical] | [%] | [Brief] |
| [Dim 2] | [OK/Concern/Critical] | [%] | [Brief] |
```

### How to Repeat Template
```markdown
## How to Repeat This Audit

To perform this audit yourself:

1. **[First check type]**:
   ```bash
   [Command to run]
   ```
   Look for: [What to evaluate]

2. **[Second check type]**:
   ```bash
   [Command to run]
   ```
   Look for: [What to evaluate]
```

---

## Common Findings by Domain

### Security Findings
- Hardcoded secrets in source
- Missing .gitignore entries
- Debug mode in production config
- Permissive CORS settings
- Missing security headers

### Dependency Findings
- Critical/high CVE in dependencies
- Unmaintained packages (12+ months)
- Major version lag (2+ versions behind)
- Excessive transitive dependencies

### Codebase Findings
- Missing README sections
- Inconsistent file structure
- No test files present
- Missing development documentation

### Standards Findings
- Mixed naming conventions
- Inconsistent test placement
- Configuration drift between files
- Documentation style variations

---

## Teaching Patterns

### Explain the "Why"
Always connect findings to real-world impact:
- Security: "An attacker could..."
- Dependencies: "If a vulnerability is discovered..."
- Codebase: "A new team member would need to..."
- Standards: "When searching for this file, you'd have to..."

### Show Alternatives
When recommending changes, show options:
```markdown
**Options**:
1. [Option A]: [Trade-offs]
2. [Option B]: [Trade-offs]
3. [Option C]: [Trade-offs]

**Recommendation**: Option A because [reasoning]
```

### Provide Concrete Examples
Use actual code from the codebase when possible:
```markdown
**Current Pattern**:
```javascript
// From src/utils/api.ts
const apiKey = "sk-abc123...";
```

**Better Pattern**:
```javascript
const apiKey = process.env.API_KEY;
```
```

---

## Audit Boundaries

### All Auditors DO:
- Read and analyze files
- Search for patterns
- Document findings with evidence
- Explain methodology
- Provide recommendations
- Teach assessment techniques

### All Auditors DON'T:
- Modify any files
- Fix issues directly
- Access external services
- Make assumptions without evidence
- Expose sensitive information

---

*Consistent audit patterns enable consistent, high-quality assessments
that teach while they evaluate.*
