# Sovereign Recursion — deployment

A 2126-stack: source-decentralized, content-addressed, edge-served. The site
is a static artifact. The architecture honors the principle: no single host
captures the deploy.

## Layers

| Layer | Purpose | Auth needed |
|---|---|---|
| **Source** | Git repo at github.com/pondata/sovereignrecursion (mirrorable; signed commits) | gh CLI auth (`pondata`) |
| **Edge HTTPS** | Cloudflare Pages deploy from the GitHub repo, custom domain `sovereignrecursion.com` | Cloudflare account |
| **Content-addressed mirror** | IPFS pin via web3.storage / Pinata (CID is the canonical content fingerprint) | IPFS pinning service token |
| **DNS binding** | A/CNAME records for HTTPS at `sovereignrecursion.com`; DNSLINK TXT for `_dnslink.sovereignrecursion.com` → IPFS CID | Registrar DNS access |
| **Provenance** | SSH-signed git commits + content-hash in HTML comment | gh `admin:public_key` (already in scope) |

## Files

```
~/sovereign-recursion/
├── index.html              ← The principle, stated. The front door.
├── essays/index.html       ← Forthcoming long-form
├── architecture/index.html ← Forthcoming engagement surface
├── assets/                 ← (empty)
├── sales/                  ← INTERNAL, gitignored
└── DEPLOY.md               ← this file
```

## Deploy steps

### 1. Source on GitHub

```bash
cd ~/sovereign-recursion
gh repo create pondata/sovereignrecursion --private --source=. --remote=origin --push
```

(Public is fine too — the source is meant to be auditable. `--public` if you
want forks visible.)

### 2. Cloudflare Pages

Done in the Cloudflare dash:

1. Workers & Pages → Create → Pages → Connect to Git
2. Select `pondata/sovereignrecursion`
3. Build settings: **none** (pure static — leave build command empty, output dir = `/`)
4. Custom domain: add `sovereignrecursion.com` (and `www.sovereignrecursion.com`)
5. Cloudflare will provision SSL automatically and ask you to update DNS

### 3. DNS at registrar

If `sovereignrecursion.com` DNS is already at Cloudflare:
- Cloudflare Pages adds the records itself when you click "Activate"

If DNS is at a different registrar (GoDaddy / Namecheap / etc):
- Add `CNAME` for apex (or ALIAS/ANAME if registrar supports) → `<your-pages-deploy>.pages.dev`
- Add `CNAME` for `www` → same target
- Or transfer DNS to Cloudflare for cleaner DNSLINK support (recommended for the 2126 stack)

### 4. IPFS pin (the 2126 layer)

Two-line version with web3.storage:

```bash
# After installing w3 CLI: npm install -g @web3-storage/w3cli
w3 login your@email.address
w3 space create sovereign-recursion
w3 up ~/sovereign-recursion --no-wrap
# Copy the returned CID (e.g. bafybeigd...)
```

Then in Cloudflare DNS for `sovereignrecursion.com`:

```
TXT  _dnslink  "dnslink=/ipfs/bafybeigd..."
```

The site is now reachable three ways:
- `https://sovereignrecursion.com/` (Cloudflare Pages)
- `https://sovereignrecursion.com.ipns.dweb.link/` (IPFS gateway, follows DNSLINK)
- `https://bafybeigd....ipfs.dweb.link/` (direct IPFS CID, immutable)

If Cloudflare Pages or web3.storage disappears in 2126, the third URL still
works as long as anyone is pinning the CID. That's the property the principle
demands.

### 5. Email routing

`justin@sovereignrecursion.com` referenced in the colophon needs to forward.
Free option: Cloudflare Email Routing (dash → Email → Routing) → forwards to
your existing inbox in 10 minutes.

### 6. Optional — signed commits

Already on SSH per `gh auth status`. To make commits PGP-verifiable as well:

```bash
git config commit.gpgsign true
# (set up signing key separately if not already)
```

The Cloudflare Pages "Verified" badge on commits is downstream of this; for a
2126 audit trail, signed commits matter more than the badge.

## What "shipped" means here

The minimum viable ship:
- [ ] Source pushed to github.com/pondata/sovereignrecursion
- [ ] Cloudflare Pages connected, deploy succeeds
- [ ] `sovereignrecursion.com` resolves, HTTPS works
- [ ] `justin@sovereignrecursion.com` forwards somewhere

The 2126-grade ship:
- [ ] Above, plus:
- [ ] IPFS CID published, DNSLINK TXT record live
- [ ] Site reachable via at least one IPFS gateway
- [ ] Source commits SSH or PGP signed
- [ ] Content hash recorded in a separate verifiable location (e.g. a git
      tag, a tweet, a witness)

The minimum ship is enough to be live. The 2126-grade ship is what makes the
artifact archival.
