# Multi-OS CI Validation

## Overview

This repository includes a comprehensive CI validation workflow that runs on multiple operating systems to ensure cross-platform compatibility of the codebase.

## Workflow Details

**File**: `.github/workflows/ci-validation.yml`

**Triggers**:
- Push to `main`, `release-**`, or `hotfix-**` branches
- Pull requests to `main` branch

**Operating Systems**:
- Ubuntu (Linux)
- macOS
- Windows

## Validation Steps

The workflow performs the following validations on each operating system:

### 1. Repository Structure Verification
Checks that all required files exist:
- `index.html`
- `game.js`
- `engine.js`
- `base.css`

### 2. JavaScript Syntax Validation
Uses Node.js to validate the syntax of JavaScript files:
- `game.js`
- `engine.js`

This ensures that the JavaScript code is syntactically correct and will parse without errors.

### 3. HTML Structure Validation
Verifies essential HTML elements:
- Checks for proper `<!DOCTYPE html>` declaration
- Verifies presence of `<canvas>` element required for the game

### 4. Common Issues Check
Scans for potential issues:
- Warns about `console.log` statements that should be removed for production
- Identifies `TODO` or `FIXME` comments

## Benefits of Multi-OS Testing

### Why Test on Multiple Operating Systems?

1. **Path Separator Differences**: Windows uses `\` while Unix-based systems use `/`
2. **Case Sensitivity**: macOS and Windows are case-insensitive by default, Linux is case-sensitive
3. **Line Endings**: Different OS use different line ending conventions (CRLF vs LF)
4. **Shell Compatibility**: Ensures bash scripts work across platforms
5. **Node.js Behavior**: Some Node.js features behave differently across platforms

### Matrix Strategy

The workflow uses GitHub Actions' matrix strategy with `fail-fast: false`, which means:
- All OS tests run in parallel
- If one OS fails, others continue to completion
- You get complete feedback about which platforms work and which don't

## Viewing Results

When the workflow runs:
1. Go to the "Actions" tab in your GitHub repository
2. Select the "Multi-OS CI Validation" workflow
3. View the results for each operating system

Each OS validation shows:
- ✅ Green checkmark if all validations pass
- ❌ Red X if any validation fails
- Detailed logs for troubleshooting

## Local Testing

You can test the validation steps locally before pushing:

```bash
# Test file structure
test -f index.html && test -f game.js && test -f engine.js && test -f base.css && echo "✓ Files exist"

# Test JavaScript syntax
node --check game.js
node --check engine.js

# Test HTML structure
grep -q "<!DOCTYPE html>" index.html && echo "✓ DOCTYPE found"
grep -q "<canvas" index.html && echo "✓ Canvas found"
```

## Extending the Workflow

To add more validations:

1. Edit `.github/workflows/ci-validation.yml`
2. Add new steps under the `steps:` section
3. Use `shell: bash` to ensure cross-platform compatibility
4. Test locally on your development machine first

Example additional validation:

```yaml
- name: Check CSS syntax
  shell: bash
  run: |
    echo "Validating CSS files..."
    # Add CSS validation commands here
    echo "CSS validation passed ✓"
```

## Troubleshooting

### Workflow Not Running?

- Check that your branch name matches the trigger patterns
- Verify `.github/workflows/ci-validation.yml` is in the repository
- Check the Actions tab to see if workflows are enabled

### OS-Specific Failures?

- Review the logs for the failing OS
- Common issues include:
  - Path separator differences
  - Case sensitivity problems
  - Shell script compatibility

### Need to Skip Validation?

If you need to skip CI validation for a specific commit (not recommended):
- Add `[skip ci]` or `[ci skip]` to your commit message
- This will skip all CI workflows for that commit

## Best Practices

1. **Always test locally first** before pushing
2. **Review CI logs** when tests fail to understand the issue
3. **Keep validations fast** to get quick feedback
4. **Use matrix strategy** to test on multiple platforms
5. **Monitor for warnings** even when tests pass

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Matrix Strategy Guide](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)
- [Node.js on GitHub Actions](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs)
