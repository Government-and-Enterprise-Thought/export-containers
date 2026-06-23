> [!IMPORTANT]
> This repository has migrated to Forgejo: https://git.gandetl.com/Government_Enterprise_Thought_Leaders/export-containers
> The GitHub copy is archived and read-only.

# Export Containers

Client-shippable container images and configuration for single-tenant
deployments of the Government & Enterprise Thought Leaders products.

On every **production release** (a `v*` git tag) of a product, its CI pushes
the released image here as a versioned package, so a client can run it
standalone without access to our build pipeline.

## Products

| Product | Image | Configure with |
|---|---|---|
| Cyber Advisory Hub | `ghcr.io/government-and-enterprise-thought/export-containers/cyber-advisory` | [`cyber-advisory/.env.sample`](cyber-advisory/.env.sample) |
| Policy Advisor (Cyber Policy) | `ghcr.io/government-and-enterprise-thought/export-containers/policy-advisor` | [`policy-advisor/.env.sample`](policy-advisor/.env.sample) |
| Cyber Assurance | `ghcr.io/government-and-enterprise-thought/export-containers/cyber-assurance` | [`cyber-assurance/.env.sample`](cyber-assurance/.env.sample) |
| Cyber Risk Assessment | `ghcr.io/government-and-enterprise-thought/export-containers/cyber-risk-assessment` | [`cyber-risk-assessment/.env.sample`](cyber-risk-assessment/.env.sample) |

All four parse the client's MongoDB connection as a single JSON blob via
`MONGODB_CONNECTION_JSON` (see each `.env.sample`).

Tags: each release is published as the version tag (e.g. `:v1.4.0`) plus
`:latest`. **Pin to a version tag in production** — `latest` moves.

## Deploy a single tenant

```bash
# 1. Pull the released image (public — no auth needed once the package is public)
docker pull ghcr.io/government-and-enterprise-thought/export-containers/cyber-advisory:v1.4.0

# 2. Copy the sample env, fill in the values (see the file's comments)
cp cyber-advisory/.env.sample cyber-advisory.env
$EDITOR cyber-advisory.env

# 3. Run it (point it at the client's MongoDB)
docker run -d --name cyber-advisory -p 8080:8080 \
  --env-file cyber-advisory.env \
  ghcr.io/government-and-enterprise-thought/export-containers/cyber-advisory:v1.4.0
```

## Requirements

- **MongoDB** reachable from the container (the only hard external dependency).
- An **Anthropic API key** for the AI features.
- Set `SINGLE_SERVER_MODE=true` so the app runs without the Temporal cluster
  (background jobs run in-process / are disabled — fine for a single box).

> The `.env.sample` files contain **placeholders only — never commit real
> secrets here** (this repo is public). Generate long random values for the
> session/pepper/token fields with e.g. `openssl rand -hex 32`.

## One-time GHCR package setup (maintainers)

The first CI export creates each package under the org. To let clients pull
without a token, make the package **public** and link it to this repo:
*Package → Package settings → Change visibility = Public*, and *Manage Actions
access → add `export-containers` with Read*.
