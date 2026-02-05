# Fix pnpm Catalog References for pnpm 8.x Compatibility

## Problem

The project uses pnpm's `catalog:` feature which requires **pnpm 9.0+**, but your build environment has **pnpm 8.6.12**.

## Solution

I've created a script to replace all `catalog:` references with actual version numbers, making the project compatible with pnpm 8.x.

## Steps to Fix

### Option 1: Run the Fix Script (Recommended)

```bash
# From the project root directory
node scripts/fix-catalog-references.mjs
```

This will:
- Find all `package.json` files in the project
- Replace all `catalog:`, `catalog:frontend`, `catalog:e2e`, `catalog:storybook`, and `catalog:sentry` references
- Use the actual version numbers from the catalog definitions
- Report which files were modified

### Option 2: Manual Review

The script has already partially fixed these critical files:
- `package.json` (root)
- `packages/workflow/package.json`
- `packages/core/package.json`
- `packages/cli/package.json`
- `packages/@n8n/ai-utilities/package.json`
- `packages/@n8n/nodes-langchain/package.json`
- `packages/nodes-base/package.json`

You can review these changes and manually update remaining files if needed.

## What Changed

1. **Root `package.json`**: Updated pnpm engine requirement from `>=9.0.0` to `>=8.6.0`
2. **Package files**: All `"catalog:"` references replaced with actual version numbers like:
   - `"catalog:"` → `"3.25.67"` (zod)
   - `"catalog:frontend"` → `"^3.5.13"` (vue)
   - `"catalog:sentry"` → `"^10.36.0"` (@sentry/node)

## After Running the Script

1. The build should now work with pnpm 8.6.12
2. You may want to delete the lock file and reinstall:
   ```bash
   rm pnpm-lock.yaml
   pnpm install
   ```

## Note

The ideal solution is to upgrade your build environment to pnpm 9.0+ to use the catalog feature properly. This fix is a workaround for environments where upgrading pnpm is not possible.
