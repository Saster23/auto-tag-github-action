# test-github-actions

🚀 Auto Tagging GitHub Action

This repository uses a GitHub Action to automatically create and push version tags in the format vX.Y.Z whenever code is pushed to the main branch.

📦 How It Works

On every push to the main branch:
The action checks all existing Git tags matching the format vX.Y.Z.
If no tags exist, it creates v1.0.0.
If tags exist, it finds the latest tag and increments the patch version (e.g., v1.0.0 → v1.0.1).
The new tag is created and pushed to GitHub.


✅ Example

If the following tags exist:

v1.0.0 (if none exists, will create this)
v1.0.1
v1.0.2

Then on the next push to main, the action will create:

v1.0.3


📂 Workflow Location

The GitHub Action is defined in:

.github/workflows/auto-tag.yml
🔐 Permissions

This uses the default GITHUB_TOKEN to push tags. If your repository uses branch protection rules or you encounter permissions issues, consider creating a Personal Access Token (PAT) with contents: write and add it as a secret (PAT_TOKEN) in your repo settings.


Then modify the push step like so:

env:
  GIT_AUTH_TOKEN: ${{ secrets.PAT_TOKEN }}
run: |
  git remote set-url origin https://x-access-token:${GIT_AUTH_TOKEN}@github.com/${{ github.repository }}
  git tag ${{ steps.tag.outputs.new_tag }}
  git push origin ${{ steps.tag.outputs.new_tag }}
🛠️ Customization

This action currently bumps only the patch version. You can extend it to support minor or major bumps based on commit messages, PR labels, or other logic.
