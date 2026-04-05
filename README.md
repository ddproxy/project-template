# project-template

Workflow templates for ddproxy repositories. Copy the `.github/` and `.gitea/` directories into a new repository to get automatic project registry integration.

## What's included

```
.github/workflows/
  upsert-project.yml   # manual dispatch — register or update this repo in the registry
  upsert-version.yml   # auto on v* tag push + manual dispatch to patch a version record
.gitea/workflows/
  upsert-project.yml   # Gitea equivalent
  upsert-version.yml   # Gitea equivalent
```

## How it works

**On every `v*` tag push**, `upsert-version` runs automatically:
- Reads `name` and `license` from `package.json` (or falls back to the repo name / LICENSE file)
- Builds a changelog from `git log` since the previous tag
- Constructs asset and tag URLs from `GITHUB_REPOSITORY` and optional Gitea variables
- Calls `registry-actions/upsert-version` to create or update the version record in the registry

**On manual dispatch**, `upsert-project` lets you register a new repo or patch any project field — description, license, repo URLs, tags — without touching version records.

`upsert-version` can also be triggered manually to patch an existing version (e.g. to add a Gitea tag URL after initial GitHub-only registration).

## Setup

1. Copy `.github/` and `.gitea/` into your repository.
2. Create the following in GitHub:

   | Name | Type | Value |
   |------|------|-------|
   | `REGISTRY_TOKEN` | Secret | PAT with write access to `ddproxy/project-registry` |
   | `GITEA_BASE_URL` | Variable | Gitea web URL, e.g. `https://git.example.com` |
   | `GITEA_ORG` | Variable | Gitea org slug, e.g. `com.example` |

3. Create the following in Gitea:

   | Name | Type | Value |
   |------|------|-------|
   | `GITEA_REGISTRY_TOKEN` | Secret | Gitea token with write access to the `registry` repo |
   | `GITHUB_MIRROR_URL` | Variable | Full GitHub repo URL, e.g. `https://github.com/ddproxy/my-repo` |

4. Run **Actions → Upsert Project** once to register the project record.
5. Tag a release — the version record is created automatically.

## Registry actions

The workflows call [registry-actions](https://github.com/ddproxy/registry-actions) at `v0.1.0`. Update the `@v0.1.0` references when a new version is released.

## License

MIT — Copyright (c) 2026 Jon West
