# GitHub Actions: Metrics Collector for Large Repositories

## Overview
This workflow dynamically scans multiple repositories in an organization, fetches metadata like topics, and handles 3000–10000+ repositories using scalable batching.

It uses **GitHub App authentication** to ensure large-scale, secure, and automated access to GitHub APIs.

---

## Why Use GitHub App Instead of Personal Access Token (PAT)?

| Factor | Personal Access Token | GitHub App |
|:------|:----------------------|:-----------|
| API Rate Limit | 15,000 calls/hour per token | 5,000 calls/hour per installation |
| Security | Tied to a human user | Independent app identity |
| Permissions | Broad (full repo access) | Fine-grained (topics only, metadata only) |
| Token Rotation | Manual | Auto (JWT and 1-hour access tokens) |
| Scalability | Difficult above 5000 repos | Designed for enterprise scaling |

**Conclusion:**  
> For 3000+ and growing repos, GitHub App is **mandatory** for sustainable scaling.

---

## How It Works

1. GitHub App private key creates a JWT.
2. JWT exchanged for an Installation Access Token.
3. Access Token used to make GitHub API calls for fetching/updating repo data.
4. Matrix batches (500 repos per job) ensure GitHub Actions API limits are respected.
5. Failed repos are retried separately without re-running all batches.

---

## Prerequisites

- GitHub App created with:
  - `Read-only` access to Repository metadata
  - `Read and Write` access to Topics
- Secrets added in GitHub Actions:
  - `GH_APP_ID`
  - `GH_APP_INSTALLATION_ID`
  - `GH_APP_PRIVATE_KEY` (base64 encoded)

---

## Workflow Highlights

- Dynamic Matrix Creation
- JSON Validation
- Automatic Token Handling
- Batch Processing of Repositories
- Retry Failed Repos
- Full GitHub Actions Summary

---

## Notes
- Batch size is adjustable (currently 500 repos per batch).
- No need to manually rotate tokens.
- Extremely low failure rate due to automated retries.
