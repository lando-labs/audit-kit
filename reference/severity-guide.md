# Severity Assessment Guide

How to assign appropriate severity levels to audit findings.

---

## Severity Levels Overview

| Level | Response Time | Examples |
|-------|--------------|----------|
| **Critical** | Immediate (today) | Exposed secrets, critical vulnerabilities |
| **High** | This week | Known CVEs, authentication issues |
| **Medium** | This month | Configuration issues, documentation gaps |
| **Low** | When convenient | Style inconsistencies, minor improvements |
| **Info** | No action needed | Observations, positive practices |

---

## Severity Decision Framework

### Critical

**Definition**: Immediate risk to security, data, or system stability.

**Characteristics**:
- Actively exploitable
- Potential for significant damage
- Requires immediate remediation
- May warrant pausing other work

**Security Examples**:
- API keys or passwords in source code
- Database credentials in public repository
- Authentication bypass vulnerabilities
- Remote code execution vulnerabilities

**Dependency Examples**:
- Critical CVE (CVSS 9.0+) with public exploit
- Compromised package (supply chain attack)

**Codebase Examples**:
- Build completely broken
- Data corruption risk

**Standards Examples**:
- (Rarely critical - would need to cause the above)

**Question to Ask**: Could this cause immediate harm if discovered by an attacker or bad actor?

---

### High

**Definition**: Significant risk requiring prompt attention.

**Characteristics**:
- Exploitable with effort
- Clear path to damage
- Should be addressed within days
- May block deployments

**Security Examples**:
- Secrets in non-public but accessible locations
- Missing authentication on sensitive endpoints
- Overly permissive access controls
- High-severity vulnerabilities (CVSS 7.0-8.9)

**Dependency Examples**:
- High CVE in production dependencies
- Unmaintained package with known issues
- Package with security advisory pending fix

**Codebase Examples**:
- Missing critical documentation (deployment, recovery)
- Tests completely absent for critical paths
- Build works but produces insecure output

**Standards Examples**:
- Configuration drift causing security gaps
- Inconsistency that causes silent failures

**Question to Ask**: Would this cause significant problems if not fixed within a week?

---

### Medium

**Definition**: Moderate risk or significant friction.

**Characteristics**:
- Requires specific circumstances to exploit
- Causes ongoing friction or risk
- Should be planned for remediation
- Acceptable to defer for higher priorities

**Security Examples**:
- Missing security headers
- Verbose error messages in production
- Debug features not disabled
- Medium vulnerabilities (CVSS 4.0-6.9)

**Dependency Examples**:
- Medium CVE in production dependencies
- One major version behind
- Package with declining maintenance

**Codebase Examples**:
- Incomplete README sections
- Tests present but incomplete coverage
- Documentation outdated but present
- Structure inconsistencies

**Standards Examples**:
- Naming convention drift (30-50% inconsistent)
- Configuration differences across environments
- Multiple patterns for same task

**Question to Ask**: Does this cause ongoing friction or accumulating risk?

---

### Low

**Definition**: Minor issues or improvement opportunities.

**Characteristics**:
- Minimal direct risk
- Small friction points
- Nice to fix but not urgent
- Can be addressed opportunistically

**Security Examples**:
- Minor configuration improvements
- Optional hardening not implemented
- Low vulnerabilities (CVSS 0.1-3.9)

**Dependency Examples**:
- Minor version lag
- Dev dependencies outdated
- Nice-to-have packages unmaintained

**Codebase Examples**:
- Missing optional documentation
- Style improvements possible
- Structure could be cleaner

**Standards Examples**:
- Minor naming inconsistencies
- Formatting variations
- Optional conventions not followed

**Question to Ask**: Is this worth mentioning but not worth prioritizing?

---

### Info

**Definition**: Observations without action required.

**Characteristics**:
- Neutral observations
- Positive practices to acknowledge
- Context for other findings
- Interesting patterns

**Examples**:
- "Test coverage appears comprehensive"
- "Dependency tree is relatively flat"
- "Documentation follows consistent style"
- "No secrets detected in scanned files"

**Question to Ask**: Is this useful context even though nothing needs to change?

---

## Context Modifiers

Severity isn't absolute - context matters:

### Environment Context

| Finding | Development | Staging | Production |
|---------|-------------|---------|------------|
| Debug mode enabled | Info | Medium | High |
| Test credentials | Low | Medium | Critical |
| Verbose logging | Low | Low | Medium |

### Exposure Context

| Finding | Private Repo | Public Repo |
|---------|--------------|-------------|
| API key in source | High | Critical |
| Outdated dependency | Medium | High |
| Missing README | Low | Medium |

### Project Context

| Finding | Active Project | Maintenance Mode | Archived |
|---------|---------------|------------------|----------|
| Outdated deps | Medium | Low | Info |
| Missing tests | High | Medium | Info |
| Doc gaps | Medium | Low | Info |

### Exploitability Context

| Factor | Increases Severity | Decreases Severity |
|--------|-------------------|-------------------|
| Public exploit | +1 level | - |
| Network accessible | +1 level | - |
| Auth required | - | -1 level |
| Complex conditions | - | -1 level |

---

## Common Severity Mistakes

### Over-Severity

**Problem**: Marking everything as Critical/High
**Impact**: Alert fatigue, real issues get ignored
**Fix**: Reserve Critical for truly immediate risks

**Signs**:
- More than 5% of findings are Critical
- Team stops reading audit reports
- "Critical" items remain unaddressed

### Under-Severity

**Problem**: Marking serious issues as Low/Info
**Impact**: Risks go unaddressed, security incidents
**Fix**: Use the "what if an attacker found this?" test

**Signs**:
- Security incidents from "Low" findings
- Issues escalate after being ignored
- External audits find higher severity

### Context-Blind Severity

**Problem**: Same severity regardless of environment
**Impact**: Wrong prioritization, wasted effort
**Fix**: Always consider production vs. development

**Signs**:
- Dev environment findings blocking work
- Production risks deprioritized

---

## Severity Communication

### For Technical Audiences

Use precise severity with full context:
```markdown
### [HIGH] SQL Injection in User Search (CVSS 7.4)

**Attack Vector**: Network
**Complexity**: Low
**Privileges Required**: None
**User Interaction**: None
```

### For Non-Technical Audiences

Translate to business impact:
```markdown
### [HIGH] User Search Security Issue

**Business Risk**: Attackers could access any user's data
**Likelihood**: Moderate (requires specific knowledge)
**Impact**: Data breach, regulatory issues
**Recommendation**: Fix before next release
```

---

## Severity Escalation

Some findings may escalate after discovery:

### When to Escalate

| Original | Escalate To | Trigger |
|----------|-------------|---------|
| High | Critical | Active exploitation discovered |
| Medium | High | New CVE published for issue |
| Low | Medium | Pattern of abuse observed |

### Document Escalation

```markdown
**Severity**: Medium -> High (Escalated 2024-01-15)
**Reason**: CVE-2024-XXXX published affecting this pattern
**Original Assessment**: Missing input validation
**Updated Assessment**: Missing input validation with known exploit
```

---

## Severity Summary Table

| Level | Action | Timeline | Examples |
|-------|--------|----------|----------|
| Critical | Stop & fix | Hours | Secrets, critical CVE, RCE |
| High | Prioritize | Days | Auth issues, high CVE |
| Medium | Plan | Weeks | Config issues, gaps |
| Low | Opportunistic | When able | Style, minor improvements |
| Info | Document | None | Observations, positives |

---

*Severity is about prioritization, not punishment. The goal is to help
teams focus effort where it matters most.*
