# Remediation Patterns Reference

Common fixes for common audit findings.

---

## Security Remediation

### Exposed Secrets

**Finding**: API key, password, or token in source code

**Immediate Actions**:
1. Rotate the exposed credential immediately
2. Remove from source code
3. Add to .gitignore if file-based

**Code Change**:
```javascript
// Before (DO NOT DO THIS)
const apiKey = "sk-abc123...";

// After (Correct Pattern)
const apiKey = process.env.API_KEY;
if (!apiKey) {
  throw new Error('API_KEY environment variable required');
}
```

**Prevention**:
```bash
# Install pre-commit hook
npm install --save-dev husky
npx husky add .husky/pre-commit "npx secretlint"

# Or use git-secrets
brew install git-secrets
git secrets --install
git secrets --register-aws
```

**Trade-offs**:
- Environment variables require deployment configuration
- Secret managers add complexity but improve security
- Pre-commit hooks slow down commits slightly

---

### Missing .gitignore Entries

**Finding**: Sensitive files not excluded from version control

**Immediate Actions**:
1. Add files to .gitignore
2. Remove from git tracking if already committed
3. Verify removal from history if sensitive

**Commands**:
```bash
# Add to .gitignore
echo ".env" >> .gitignore
echo "*.pem" >> .gitignore
echo "credentials.json" >> .gitignore

# Remove from tracking (keep local file)
git rm --cached .env

# For sensitive files already in history
# Use BFG Repo-Cleaner or git filter-branch
# Warning: This rewrites history
```

**Standard .gitignore Additions**:
```gitignore
# Environment files
.env
.env.local
.env.*.local

# Credentials
*.pem
*.key
credentials.json
service-account.json

# IDE
.idea/
.vscode/
*.swp

# Build outputs
dist/
build/
node_modules/
```

---

### Debug Mode in Production Config

**Finding**: Debug features enabled in production configuration

**Configuration Changes**:
```javascript
// Before
const config = {
  debug: true,
  logLevel: 'verbose'
};

// After
const config = {
  debug: process.env.NODE_ENV === 'development',
  logLevel: process.env.LOG_LEVEL || 'warn'
};
```

**Environment-Based Configuration**:
```typescript
// config.ts
const isProduction = process.env.NODE_ENV === 'production';

export const config = {
  debug: !isProduction,
  logLevel: isProduction ? 'error' : 'debug',
  showStackTraces: !isProduction,
  minifyAssets: isProduction
};
```

---

## Dependency Remediation

### Vulnerable Dependency

**Finding**: Known CVE in a dependency

**Assessment First**:
```bash
# Check if exploitable in your context
npm audit
# Note the severity and attack vector
```

**Update Options**:
```bash
# If direct dependency - update to fixed version
npm install package-name@fixed-version

# If transitive dependency - update parent
npm update parent-package

# If no fix available - override (use carefully)
# In package.json:
{
  "overrides": {
    "vulnerable-package": "fixed-version"
  }
}
```

**If Update Breaks Things**:
```bash
# Check for breaking changes
npm outdated package-name

# Read changelog
# Test thoroughly before deploying

# If truly blocked, document risk acceptance
# Create issue to track future remediation
```

---

### Unmaintained Package

**Finding**: Package hasn't been updated in 12+ months

**Options**:

1. **Find Alternative**:
   ```bash
   # Search for maintained alternatives
   npm search keyword --long
   # Check npm trends: https://npmtrends.com/
   ```

2. **Fork and Maintain**:
   ```bash
   # If critical and no alternatives
   # Fork to your org, apply patches
   npm install github:your-org/forked-package
   ```

3. **Accept Risk** (document):
   ```markdown
   ## Risk Acceptance: unmaintained-package

   **Date**: YYYY-MM-DD
   **Package**: unmaintained-package@1.2.3
   **Risk**: No security updates if vulnerability discovered
   **Mitigation**: Package is isolated, has no network access
   **Review Date**: YYYY-MM-DD (6 months)
   ```

---

### Major Version Lag

**Finding**: 2+ major versions behind current

**Gradual Update Strategy**:
```bash
# Check what would change
npm outdated

# Update one major at a time
npm install package@2.0.0  # from 1.x
# Test thoroughly
npm install package@3.0.0  # from 2.x
# Test thoroughly
```

**Breaking Change Investigation**:
```bash
# Read changelogs for each major version
# Search for migration guides
# Check for codemods (automated migration)
npx @package/codemod --transform migration-name
```

---

## Codebase Remediation

### Missing README Sections

**Finding**: README lacks essential information

**Essential README Template**:
```markdown
# Project Name

Brief description of what this project does.

## Prerequisites

- Node.js 18+
- npm or yarn
- (Other requirements)

## Installation

\`\`\`bash
git clone <repo-url>
cd project-name
npm install
\`\`\`

## Environment Setup

\`\`\`bash
cp .env.example .env
# Edit .env with your values
\`\`\`

### Required Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `DATABASE_URL` | Database connection | `postgres://...` |
| `API_KEY` | External API key | `sk-...` |

## Running Locally

\`\`\`bash
npm run dev
\`\`\`

Visit `http://localhost:3000`

## Running Tests

\`\`\`bash
npm test
\`\`\`

## Deployment

[Link to deployment documentation]

## Contributing

[Link to CONTRIBUTING.md or brief guidelines]

## License

[License type]
```

---

### Missing Tests

**Finding**: No test files or test infrastructure

**Quick Test Setup (JavaScript/TypeScript)**:
```bash
# Install test framework
npm install --save-dev vitest @testing-library/react

# Create test config
# vitest.config.ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'jsdom',
    globals: true
  }
});

# Add test script
# package.json
{
  "scripts": {
    "test": "vitest"
  }
}
```

**First Test to Write**:
```typescript
// src/utils/__tests__/example.test.ts
import { describe, it, expect } from 'vitest';
import { yourFunction } from '../example';

describe('yourFunction', () => {
  it('should return expected result', () => {
    expect(yourFunction('input')).toBe('expected');
  });
});
```

**Test Coverage Targets**:
- Start with critical paths (authentication, payments)
- Add tests for bugs as they're fixed
- Build toward 70% coverage over time

---

### Inconsistent File Structure

**Finding**: Similar files organized differently

**Restructuring Approach**:
```bash
# 1. Document the target structure
# 2. Create migration script or checklist
# 3. Move files in batches
# 4. Update imports (use editor refactoring)
# 5. Verify nothing broke
npm test
npm run build
```

**Automated Import Fixes**:
```bash
# VSCode: "Organize Imports" on save
# Or use eslint-plugin-import

# eslint config
{
  "plugins": ["import"],
  "rules": {
    "import/order": ["error", {
      "groups": ["builtin", "external", "internal"]
    }]
  }
}
```

---

## Standards Remediation

### Naming Convention Inconsistency

**Finding**: Mixed naming patterns for similar files

**Batch Rename Script**:
```bash
# Rename kebab-case to PascalCase
for file in src/components/*.tsx; do
  # Convert kebab-case to PascalCase
  new_name=$(echo "$file" | sed -E 's/-([a-z])/\U\1/g' | sed 's/^./\U&/')
  git mv "$file" "$new_name"
done
```

**Enforce Going Forward**:
```json
// .eslintrc.json
{
  "rules": {
    "@typescript-eslint/naming-convention": [
      "error",
      {
        "selector": "default",
        "format": ["camelCase"]
      },
      {
        "selector": "variable",
        "format": ["camelCase", "UPPER_CASE"]
      },
      {
        "selector": "typeLike",
        "format": ["PascalCase"]
      }
    ]
  }
}
```

---

### Configuration Drift

**Finding**: Different settings in similar config files

**Consolidation Approach**:
```javascript
// Create shared base config
// eslint.base.js
module.exports = {
  rules: {
    // Shared rules
  }
};

// Extend in each config
// .eslintrc.js
module.exports = {
  extends: ['./eslint.base.js'],
  // Project-specific overrides only
};
```

**Monorepo Pattern**:
```json
// Root package.json
{
  "workspaces": ["packages/*"],
  "devDependencies": {
    // Shared dev dependencies
  }
}
```

---

### Inconsistent Error Handling

**Finding**: Multiple patterns for handling errors

**Create Shared Utilities**:
```typescript
// src/utils/errors.ts
export class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number = 500
  ) {
    super(message);
    this.name = 'AppError';
  }
}

export async function withErrorHandler<T>(
  fn: () => Promise<T>,
  context: string
): Promise<T> {
  try {
    return await fn();
  } catch (error) {
    console.error(`Error in ${context}:`, error);
    throw error;
  }
}

// Usage
const data = await withErrorHandler(
  () => fetchUserData(userId),
  'fetchUserData'
);
```

---

## Remediation Planning

### Prioritization Matrix

| Impact | Effort Low | Effort Medium | Effort High |
|--------|------------|---------------|-------------|
| High | Do Now | Do Now | Plan & Resource |
| Medium | Do Now | Schedule | Backlog |
| Low | Opportunistic | Backlog | Skip/Revisit |

### Batch vs. Incremental

**Batch Remediation** (dedicated sprint):
- Large structural changes
- Breaking changes
- Requires coordinated testing
- Examples: Major dependency updates, restructuring

**Incremental Remediation** (ongoing):
- Style fixes
- Documentation additions
- Non-breaking improvements
- Examples: Adding tests, fixing naming

### Documentation of Changes

```markdown
## Remediation Record

### Change: [Description]
**Date**: YYYY-MM-DD
**Finding**: [Link to audit finding]
**Severity**: [Original severity]

**What Changed**:
- [File/config changed]
- [Nature of change]

**Verification**:
- [ ] Tests pass
- [ ] Build succeeds
- [ ] Deployed to staging
- [ ] Verified in staging

**Rollback Plan**:
[How to undo if needed]
```

---

*Remediation should be planned, prioritized, and documented. Not every
finding needs immediate action - the goal is systematic improvement.*
