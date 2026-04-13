# devops-templates

Reusable GitHub Actions AI pipeline for all company repositories.
Powered by **Google Gemini 2.0 Flash (free tier)** — no credit card required.

---

## What it does

Every pull request automatically gets:

| Feature | Description |
|---|---|
| 📝 PR Description | Auto-generates a structured PR description if empty |
| 🔍 Code Review | Reviews diff for bugs, quality issues, best practices |
| 🔐 Security Scan | Scans for vulnerabilities specific to each repo type |
| 🧪 Test Suggestions | Suggests tests to write for changed code |

Each workflow is **repo-type aware** — prompts are tailored for Frontend, .NET, and Terraform.

---

## Setup (one-time, ~5 minutes)

### 1. Get a free Gemini API key
- Go to [aistudio.google.com](https://aistudio.google.com)
- Sign in with your Google account (no credit card needed)
- Click **Get API key** → **Create API key**
- Copy the key

### 2. Add org-level secret in GitHub
- Go to your GitHub org: `Settings → Secrets and variables → Actions`
- Click **New organization secret**
- Name: `GEMINI_API_KEY`
- Value: your Gemini API key
- Access: All repositories (or select specific ones)

### 3. Add caller workflow to each repo
Copy the relevant file from `caller-workflows/` into your repo at `.github/workflows/ai-pipeline.yml`

| Repo type | Caller file to copy |
|---|---|
| Frontend (React/Vue/etc.) | `caller-workflows/frontend-ai-pipeline.yml` |
| Backend (.NET) | `caller-workflows/dotnet-ai-pipeline.yml` |
| Infrastructure (Terraform) | `caller-workflows/terraform-ai-pipeline.yml` |

### 4. Replace YOUR-ORG
In each caller file, replace `YOUR-ORG` with your actual GitHub organization name:

```yaml
uses: YOUR-ORG/devops-templates/.github/workflows/ai-pr-review.yml@main
#     ^^^^^^^^ replace this with your org name
```

---

## Repository structure

```
devops-templates/
├── .github/
│   └── workflows/
│       ├── ai-pr-review.yml          # Reusable: code review
│       ├── ai-pr-description.yml     # Reusable: PR description
│       ├── ai-security-scan.yml      # Reusable: security scan
│       └── ai-test-suggestions.yml   # Reusable: test suggestions
├── caller-workflows/
│   ├── frontend-ai-pipeline.yml      # Copy to FE repos
│   ├── dotnet-ai-pipeline.yml        # Copy to .NET repos
│   └── terraform-ai-pipeline.yml     # Copy to Terraform repos
└── README.md
```

---

## Cost

Using Google Gemini free tier:
- **1,500 requests/day** — completely free
- **No credit card required**
- Model: `gemini-2.0-flash` — fast and capable
- Estimated cost for typical team: **$0/month**

---

## Customizing prompts

Each reusable workflow has a step where prompts are customized per repo type.
Edit these to match your team's coding standards. Example in `ai-pr-review.yml`:

```yaml
if [ "$REPO_TYPE" = "dotnet" ]; then
  EXTRA="Focus on: async/await misuse, SOLID violations..."
fi
```
