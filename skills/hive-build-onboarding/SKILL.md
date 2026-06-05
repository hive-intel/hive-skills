---
name: hive-build-onboarding
description: Use this skill when the user needs a Hive API key, cannot find a key, is signing up, or needs browser PKCE versus headless dashboard setup. Guide key creation without exposing secrets in chat, logs, screenshots, or committed files.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "setup"
  requires_network: "true"
version: 1.0.0
---

# hive-build-onboarding — Get a Hive API Key

Use this skill when the user says any of:

- "I don't have a Hive key"
- "How do I sign up?"
- "I lost my API key"
- "Set up Hive for me"

Hive uses passwordless magic-link auth — no username/password, no
credit card to start. Free Demo tier: 10,000 credits/month, 30
req/min.

## Path 1 — browser sign-in (preferred)

If the user has a browser available on the same machine as their
terminal:

```bash
npx -y -p hive-intelligence@latest hive init --browser
```

What happens:

1. The CLI generates a random `state` token (CSRF guard) and starts a
   localhost listener on a random port (`127.0.0.1`).
2. It opens the user's browser at
   `https://hiveintelligence.xyz/auth/cli?callback_port=<port>&state=<state>`.
3. The user signs in with magic-link (one-time email link, no
   password). If they don't have an account, one is created.
4. The site issues a fresh API key and redirects the browser to
   `http://127.0.0.1:<port>/callback?key=<key>&email=<email>&state=<state>`.
5. The CLI verifies the returned `state` matches (rejects on mismatch),
   then stores the key in `~/.config/hive/credentials.json` with `0600`
   permissions (override the directory with `HIVE_CONFIG_DIR`).
6. The browser tab shows an "Authenticated" page the user can close.

The key never lives on the clipboard. If a step fails, the CLI
surfaces the exact error (port collision, browser refused to open,
`state` mismatch, or a 5-minute timeout).

## Path 2 — Dashboard copy-paste (fallback for headless)

If `init --browser` won't work (CI environment, SSH session, Docker
container, restricted network):

1. From any machine with a browser, open
   `https://www.hiveintelligence.xyz/dashboard/keys`.
2. Click "Sign in" and enter the user's email.
3. Open the magic-link email and click through. Account is created
   automatically if it doesn't exist.
4. Click "Create key" → name it (e.g., "production-CI") → copy the
   shown secret.
5. Set `HIVE_API_KEY=hive_live_…` in the headless environment's env.

For CI, store the key in the secret manager (GitHub Secrets, Vault,
1Password CLI, etc.) and inject as `HIVE_API_KEY` at runtime.

## Key prefixes

The first three characters of the secret tell you the environment:

- `hive_live_` — production key
- `hive_test_` — test mode (no rate limiting against the user's quota,
  capped tools)
- `hive_dev_` — local development key

The user can have multiple keys. Plan-tier limits: Demo = 1 key,
Analyst = 10, Pro = 25, Enterprise = 100.

## When the user can't find an old key

Hive **never reveals a key after the creation dialog closes**. The
secret is hashed server-side; only a prefix is stored. If the user
lost a key:

1. Don't try to "recover" it — that's cryptographically impossible.
2. Direct them to revoke it (so it can't be used by whoever has it
   now): https://www.hiveintelligence.xyz/dashboard/keys → revoke.
3. Create a new key. Update wherever the old one was used.

This is the same pattern Stripe / Linear / GitHub use. It's a feature,
not a UX bug.

## Verifying the new key works

After Path 1 or Path 2:

```bash
hive doctor
# or
curl -H "Authorization: Bearer $HIVE_API_KEY" \
  https://mcp.hiveintelligence.xyz/api/v1/tools?limit=1
```

A successful `doctor` reports its `HIVE_API_KEY` and `Server health`
checks as OK. A successful curl returns `{"ok": true, "data": [...], ...}`.
If you see HTTP 401 or JSON-RPC `-32001`, the key is wrong or
disabled.

## Plan upgrade

If the user wants higher limits than the Demo tier:

- Analyst — $129/month, 500k credits, 500 req/min, 10 keys
- Pro — $499/month, 2M credits, 1k req/min, 25 keys
- Enterprise — custom, unlimited credits, 3k req/min, 100 keys

Direct them to https://www.hiveintelligence.xyz/dashboard/plans.
Today the upgrade flow is mailto-based; in-app Stripe checkout is on
the roadmap.

## Runtime status handling

After onboarding, Hive tools may still report `missing_key`, `plan_required`,
`rate_limited`, `degraded`, or `failing` for provider-specific runtime states.
Do not create a new Hive key for provider plan gates; explain the blocked
provider/tool and the upgrade or retry path.

## Reference

- Authentication docs: https://www.hiveintelligence.xyz/authentication
- Dashboard keys: https://www.hiveintelligence.xyz/dashboard/keys
- Public agent manifest: https://www.hiveintelligence.xyz/agent-onboarding/SKILL.md
