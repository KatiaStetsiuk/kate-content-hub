# Kate's Content Hub

A Mintlify-powered personal reference site for content produced by the AI content factory.

## Structure

```
kate-content-hub/
├── docs.json                          # Mintlify config + navigation
├── introduction.mdx                   # Home page
├── linkedin-posts/
│   ├── ai-economics/                  # Posts: AI as Economics pillar
│   ├── operating-models/              # Posts: Operating Models & Governance pillar
│   └── scaling-beyond-experiments/    # Posts: Scaling Beyond Experiments pillar
├── articles/
│   ├── ai-economics/
│   ├── operating-models/
│   └── scaling-beyond-experiments/
├── topic-ideas/
│   ├── ai-economics/
│   ├── operating-models/
│   └── scaling-beyond-experiments/
├── content-calendar/
└── .github/workflows/
    └── publish-content.yml            # Auto-push workflow
```

## Setup (one-time)

### 1. Connect to Mintlify

1. Go to [mintlify.com](https://mintlify.com) and create an account
2. Create a new docs project → select "Connect GitHub repo"
3. Point it at this repository
4. Mintlify will auto-deploy on every push to `main`

Your docs will be live at `your-project.mintlify.app` (or a custom domain if you configure one).

### 2. Enable the GitHub Action

The workflow at `.github/workflows/publish-content.yml` lets you push new content files without touching code.

**To publish a new piece of content:**

Option A — GitHub UI:
1. Go to your repo → Actions → "Publish Content to Hub"
2. Click "Run workflow"
3. Fill in `file_path` and `file_content`
4. Run

Option B — GitHub API (for future automation):
```bash
curl -X POST \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/YOUR_USERNAME/YOUR_REPO/dispatches \
  -d '{
    "event_type": "publish-content",
    "client_payload": {
      "file_path": "linkedin-posts/ai-economics/my-post.mdx",
      "file_content": "---\ntitle: My Post\n---\n\nContent here...",
      "commit_message": "Add: pilot trap post"
    }
  }'
```

### 3. Adding new pages to navigation

When you add a new content file, register it in `docs.json` under the relevant navigation group.

Example — adding a LinkedIn post:
```json
{
  "group": "AI as Economics",
  "pages": [
    "linkedin-posts/ai-economics/index",
    "linkedin-posts/ai-economics/your-new-post"
  ]
}
```

## Content file naming convention

| Type | Path pattern | Example |
|------|-------------|---------|
| LinkedIn post | `linkedin-posts/[pillar]/[slug].mdx` | `linkedin-posts/ai-economics/pilot-trap.mdx` |
| Article | `articles/[pillar]/[slug].mdx` | `articles/operating-models/caio-mistake.mdx` |
| Topic idea | `topic-ideas/[pillar]/[slug].mdx` | `topic-ideas/scaling/mckinsey-response.mdx` |
| Calendar | `content-calendar/[YYYY-MM].mdx` | `content-calendar/2026-04.mdx` |

**Pillar slugs:**
- `ai-economics`
- `operating-models`
- `scaling-beyond-experiments`

## Updating the skill

The content factory skill lives separately in your Claude Project. When the skill produces content, 
save it here manually (or via the GitHub Action) using the templates in each section's `index.mdx`.
