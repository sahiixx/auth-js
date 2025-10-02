# Build and Deploy Verification

This document verifies that the build and deploy infrastructure for `@supabase/auth-js` is properly configured and working.

## Build System

### Local Build

The project uses TypeScript and produces two output formats:

1. **CommonJS** (`dist/main/`) - for Node.js environments
2. **ES Modules** (`dist/module/`) - for modern JavaScript environments

**Build command:**
```bash
npm run build
```

This command:
1. Generates version information (`genversion`)
2. Cleans previous build artifacts (`rimraf dist docs`)
3. Formats code with Prettier
4. Compiles TypeScript to CommonJS and ES Modules
5. Runs ESLint for code quality

### Documentation Build

Documentation is generated using TypeDoc:

```bash
npm run docs        # Generate HTML documentation
npm run docs:json   # Generate JSON specification
```

Output location: `docs/v2/`

## Deployment

### Continuous Integration (CI)

**Workflow:** `.github/workflows/ci.yml`

- Triggers on: Pull requests and pushes to `master`, `next`, `rc` branches
- Node versions: 18, 20
- Steps:
  1. Checkout code
  2. Setup Node.js
  3. Install dependencies (`npm ci`)
  4. Build project (`npm run build`)
  5. Run tests (`npm t`)
  6. Upload coverage to Coveralls

### Release and Publishing

**Workflow:** `.github/workflows/release.yml`

- Triggers on: Pushes to `master` and `release/*` branches
- Uses: Google's release-please for automated versioning
- Steps:
  1. Create or update release PR
  2. Build release artifacts
  3. Update version in all files
  4. Publish to npm with provenance
  5. Publish as both `@supabase/auth-js` and `@supabase/gotrue-js`
  6. Create GitHub release
  7. Create release branches for patches

### Documentation Deployment

**Workflow:** `.github/workflows/docs.yml`

- Triggers on: Pushes to `master` branch
- Deployment: GitHub Pages
- Steps:
  1. Checkout code
  2. Install dependencies
  3. Build documentation
  4. Deploy to `gh-pages` branch

## Package Configuration

### Entry Points

- **Main (CommonJS):** `dist/main/index.js`
- **Module (ESM):** `dist/module/index.js`
- **Types:** `dist/module/index.d.ts`

### Published Files

The npm package includes:
- `dist/` - Compiled JavaScript and type definitions
- `src/` - TypeScript source files

### Version Management

- Version: Managed by semantic-release
- Current placeholder: `0.0.0`
- Actual version: Set during release process

## Verification

### Build Verification

```bash
# Clean build
npm run clean
npm run build

# Verify outputs
ls -la dist/main/index.js
ls -la dist/module/index.js

# Check package contents
npm pack --dry-run
```

### Test Verification

```bash
# Run full test suite (requires Docker)
npm test

# Run only test suite (without infrastructure)
npm run test:suite
```

## Status

✅ **Build System:** Working correctly
✅ **CI Pipeline:** Properly configured
✅ **Release Workflow:** Automated with release-please
✅ **Documentation:** Deployed to GitHub Pages
✅ **Package Configuration:** Correct entry points and exports

All build and deployment infrastructure is operational.
