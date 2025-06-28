# GitHub Actions Status Update System - Setup Guide

This system automates weekly status updates for GitHub Issues using GitHub Pages and GitHub Actions.

## 🚀 Quick Setup

### 1. Repository Setup

1. **Create/use a GitHub repository** for your project
2. **Enable GitHub Pages** in repository settings:
   - Go to Settings → Pages
   - Source: "Deploy from a branch"
   - Branch: `main` (or your default branch)
   - Folder: `/ (root)`

### 2. Add Files to Your Repository

Create these files in your repository:

```
your-repo/
├── .github/
│   └── workflows/
│       ├── process-status-updates.yml
│       ├── generate-weekly-form.yml (optional)
│       └── deploy-form.yml (optional)
├── index.html (copy the status form HTML)
└── README.md
```

### 3. Configure the Status Form

1. **Save the HTML form** as `index.html` in your repository root
2. **Update the form defaults**:
   - Change `your-username` to your GitHub username
   - Change `my-project` to your repository name
   - Change `your-org` to your organization name

### 4. Set Up GitHub Actions

1. **Copy the workflow files** to `.github/workflows/` directory
2. **Commit and push** to trigger the workflows
3. **Check Actions tab** to ensure workflows are enabled

## 🔧 How It Works

### Workflow Overview

1. **Team members visit** the GitHub Pages form (e.g., `https://your-username.github.io/your-repo`)
2. **Form loads** their assigned open issues automatically
3. **They update status** and add comments for each issue
4. **Form submission creates** a special "Weekly Status Update" issue
5. **GitHub Action processes** the update issue and:
   - Adds comments to the original issues
   - Updates issue labels based on status
   - Closes completed issues
   - Closes the status update issue

### Status Options

- 🔄 **In Progress**: Issue is being worked on
- ✅ **Completed**: Issue is finished (will close the issue)
- 🚫 **Blocked**: Issue is blocked by dependencies
- 👀 **Needs Review**: Issue is ready for review

## 📋 Usage Instructions

### For Team Members

1. **Visit the form**: Go to your GitHub Pages URL
2. **Enter credentials**: Your GitHub username and repo details
3. **Load issues**: Click "Load My Issues" to fetch assigned issues
4. **Update status**: Select status and add comments for each issue
5. **Submit**: Click "Submit Status Updates"

### For Project Managers

1. **Monitor updates**: Check the GitHub Actions tab for processing logs
2. **Review changes**: Updated issues will have new comments and labels
3. **Set up reminders**: Enable the weekly reminder workflow (optional)

## 🛠 Customization Options

### Status Labels

The system automatically adds/removes these labels:
- `status:in-progress`
- `status:completed`
- `status:blocked`
- `status:needs-review`

You can modify the labels in the workflow file under `getLabelsForStatus()`.

### Weekly Reminders

Enable the `generate-weekly-form.yml` workflow to:
- Automatically create reminder issues every Monday
- List all team members and their assigned issues
- Include direct links to the status form

### Form Customization

Modify `index.html` to:
- Add custom status options
- Change the styling/branding
- Add additional fields (priority, estimated completion, etc.)

## 🔐 Security & Permissions

### Required Permissions

- **Team members need**: Write access to the repository (to create issues)
- **Actions need**: Default `GITHUB_TOKEN` permissions (automatically provided)

### Data Flow

1. Form data → Status update issue (public in your repo)
2. GitHub Action → Processes and updates target issues
3. No external services or API keys required

## 🐛 Troubleshooting

### Common Issues

**Issues not loading in form:**
- Check repository visibility (public repos work better)
- Verify username/repo/owner fields are correct
- Check browser console for API errors

**GitHub Action not triggering:**
- Ensure issue title contains "Weekly Status Update"
- Check Actions are enabled in repository settings
- Verify workflow file syntax (YAML is sensitive to indentation)

**Updates not applying to issues:**
- Check the GitHub Action logs in the Actions tab
- Ensure the user has write permissions to the repository
- Verify issue numbers exist and are open

### Debug Mode

Add this to the workflow for more detailed logging:

```yaml
- name: Debug
  run: |
    echo "Issue title: ${{ github.event.issue.title }}"
    echo "Issue body: ${{ github.event.issue.body }}"
```

## 🚀 Advanced Features

### Integration with GitHub Projects

Add this to the workflow to update project cards:

```javascript
// Update project card status
await github.rest.projects.updateCard({
  card_id: cardId,
  note: `Updated: ${status}`
});
```

### Slack Notifications

Add Slack webhook integration:

```yaml
- name: Notify Slack
  uses: 8398a7/action-slack@v3
  with:
    status: success
    text: "Weekly status updates processed!"
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

### Email Summaries

Generate weekly email summaries using the GitHub API and a service like SendGrid.

## 📈 Benefits

- ✅ **No external dependencies**: Everything runs on GitHub
- ✅ **Automatic processing**: No manual intervention needed
- ✅ **Audit trail**: All updates are tracked in issue history
- ✅ **Team ownership**: Updates come from team members' accounts
- ✅ **Flexible**: Easy to customize for your workflow

## 🤝 Next Steps

1. **Test the system** with a few sample issues
2. **Train your team** on the new workflow
3. **Monitor adoption** and gather feedback
4. **Customize** the form and workflows based on your needs
5. **Consider adding** advanced features like project integration

The system is designed to be minimal but extensible - start simple and add features as needed!