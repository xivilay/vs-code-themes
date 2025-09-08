# Publishing Guide

This guide explains how to set up automatic publishing for your VS Code extension using GitHub Actions.

## Prerequisites

1. **VS Code Marketplace Account**: You need a publisher account on the VS Code Marketplace
2. **Personal Access Token (PAT)**: You need to create a PAT for publishing

## Setup Steps

### 1. Create VS Code Marketplace Publisher Account

1. Go to [Visual Studio Marketplace](https://marketplace.visualstudio.com/manage)
2. Sign in with your Microsoft account
3. Create a new publisher (this will be your `publisher` in `package.json`)
4. Note down your publisher ID

### 2. Update package.json

Update the following fields in your `package.json`:

```json
{
  "publisher": "YOUR_PUBLISHER_ID",
  "repository": {
    "type": "git",
    "url": "https://github.com/YOUR_USERNAME/vs-code-themes.git"
  },
  "homepage": "https://github.com/YOUR_USERNAME/vs-code-themes",
  "bugs": {
    "url": "https://github.com/YOUR_USERNAME/vs-code-themes/issues"
  }
}
```

### 3. Create Personal Access Token (PAT)

1. Go to [Visual Studio Marketplace](https://marketplace.visualstudio.com/manage)
2. Click on your publisher name
3. Go to "Security" tab
4. Click "Create" under "Personal Access Tokens"
5. Give it a name (e.g., "GitHub Actions Publishing")
6. Set expiration (recommend 1 year)
7. Copy the generated token (you won't see it again!)

### 4. Add GitHub Secret

1. Go to your GitHub repository
2. Click on "Settings" tab
3. Go to "Secrets and variables" → "Actions"
4. Click "New repository secret"
5. Name: `VSCE_PAT`
6. Value: Paste your Personal Access Token from step 3
7. Click "Add secret"

### 5. Test the Workflow

1. Create a new tag and push it:
   ```bash
   git tag v0.0.3
   git push origin v0.0.3
   ```

2. Check the Actions tab in your GitHub repository to see the workflow running

## Workflow Details

The workflow (`.github/workflows/publish.yml`) will:

1. Trigger when you push a tag starting with `v` (e.g., `v1.0.0`, `v0.0.3`)
2. Install dependencies and build tools
3. Package your extension using `vsce`
4. Publish to the VS Code Marketplace
5. Create a GitHub release with the tag

## Version Management

- Update the version in `package.json` before creating a tag
- Use semantic versioning (e.g., `v1.0.0`, `v1.0.1`, `v1.1.0`)
- The workflow will automatically publish when you push a tag

## Troubleshooting

### Common Issues

1. **Publisher ID mismatch**: Make sure the `publisher` field in `package.json` matches your marketplace publisher ID
2. **Invalid PAT**: Ensure your Personal Access Token is valid and has publishing permissions
3. **Version already exists**: Make sure you increment the version number before creating a new tag

### Manual Publishing (if needed)

If you need to publish manually:

```bash
npm install -g @vscode/vsce
vsce login YOUR_PUBLISHER_ID
vsce package
vsce publish
```

## Security Notes

- Never commit your Personal Access Token to the repository
- The token is stored securely in GitHub Secrets
- Rotate your token periodically for security
