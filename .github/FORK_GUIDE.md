# Fork Guide - CI/CD Configuration

## CI/CD in Forks

By default, CI/CD workflows are **disabled in forks** to conserve GitHub Actions resources and provide a cleaner fork experience.

### Why This Approach?

- **Resource efficiency**: Prevents unnecessary GitHub Actions usage across 1,600+ forks
- **Clean fork experience**: No failed workflow notifications in your fork
- **Full control**: Enable CI/CD only when you actually need it
- **PR validation**: Your changes are still fully tested when submitting PRs to the main repository

## Enabling CI/CD in Your Fork

If you need to run CI/CD workflows in your fork, follow these steps:

1. Navigate to your fork's **Settings** tab
2. Go to **Secrets and variables** → **Actions** → **Variables**
3. Click **New repository variable**
4. Create a new variable:
   - **Name**: `ENABLE_CI_IN_FORK`
   - **Value**: `true`
5. Click **Add variable**

That's it! CI/CD workflows will now run in your fork.

## Disabling CI/CD Again

To disable CI/CD workflows in your fork, you can either:

- **Delete the variable**: Remove the `ENABLE_CI_IN_FORK` variable entirely, or
- **Set to false**: Change the `ENABLE_CI_IN_FORK` value to `false`

## Alternative Testing Options

You don't always need to enable CI/CD in your fork. Here are alternatives:

### Local Testing

Run tests locally before pushing:

```bash
# Install dependencies
npm ci

# Run linting
npm run lint

# Run format check
npm run format:check

# Run validation
npm run validate

# Build the project
npm run build
```

### Pull Request CI

When you open a Pull Request to the main repository:

- All CI/CD workflows automatically run
- You get full validation of your changes
- No configuration needed

### GitHub Codespaces

Use GitHub Codespaces for a full development environment:

- All tools pre-configured
- Same environment as CI/CD
- No local setup required

## Automatic Fork Syncing

Your fork can automatically stay up to date with the upstream repository!

### How It Works

This repository includes an automatic fork sync workflow that:

- **Runs daily** at 2 AM UTC to check for upstream changes
- **Can be triggered manually** via the Actions tab (Sync Fork with Upstream → Run workflow)
- **Only runs in forks**, not in the original repository
- **Automatically merges** upstream changes if there are no conflicts

### Enabling Automatic Sync

The sync workflow is already included in your fork! It will:

1. Automatically run on schedule (daily)
2. Check for updates from the upstream repository
3. Merge changes if there are no conflicts
4. Push the updates to your fork's default branch

### Manual Sync

To manually sync your fork at any time:

1. Go to the **Actions** tab in your fork
2. Select **"Sync Fork with Upstream"** workflow
3. Click **"Run workflow"**
4. Click the green **"Run workflow"** button

### Handling Merge Conflicts

If automatic syncing fails due to merge conflicts:

1. You'll see a failed workflow run in the Actions tab
2. Sync manually using Git:
   ```bash
   git remote add upstream https://github.com/ekumachidi/BMAD-METHOD.git
   git fetch upstream
   git merge upstream/main
   # Resolve any conflicts
   git push origin main
   ```

### Alternative: GitHub's Built-in Sync

You can also use GitHub's built-in fork sync button:

1. Go to your fork's main page
2. Click **"Sync fork"** button (appears when upstream has changes)
3. Click **"Update branch"**

## Frequently Asked Questions

### Q: Will my PR be tested even if CI is disabled in my fork?

**A:** Yes! When you open a PR to the main repository, all CI/CD workflows run automatically, regardless of your fork's settings.

### Q: Can I selectively enable specific workflows?

**A:** The `ENABLE_CI_IN_FORK` variable enables all workflows. For selective control, you'd need to modify individual workflow files.

### Q: Do I need to enable CI in my fork to contribute?

**A:** No! Most contributors never need to enable CI in their forks. Local testing and PR validation are sufficient for most contributions.

### Q: Will disabling CI affect my ability to merge PRs?

**A:** No! PR merge requirements are based on CI runs in the main repository, not your fork.

### Q: Why was this implemented?

**A:** With over 1,600 forks of BMAD-METHOD, this saves thousands of GitHub Actions minutes monthly while maintaining code quality standards.

## Need Help?

- Join our [Discord Community](https://discord.gg/gk8jAdXWmj) for support
- Check the [Contributing Guide](../README.md#contributing) for more information
- Open an issue if you encounter any problems

---

> 💡 **Pro Tip**: This fork-friendly approach is particularly valuable for projects using AI/LLM tools that create many experimental commits, as it prevents unnecessary CI runs while maintaining code quality standards.
