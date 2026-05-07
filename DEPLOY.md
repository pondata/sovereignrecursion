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

### 4. IPFS pin — **deliberately not used**

**Decision (2026-05-07):** No IPFS layer.

Attempted via web3.storage; the Protocol Labs identity service had the
operator's email on a blocklist for opaque reasons traceable to a years-old
adverse interaction with someone now associated with PL. Rather than
appeal-and-pay-for-the-block-to-be-removed, we routed around the dependency
entirely.

PL is the foundational org behind IPFS, libp2p, and most of the surrounding
ecosystem. Using a non-PL pinning service (Pinata etc.) would still keep us
on PL-built protocol layers. Using a non-IPFS decentralized-storage protocol
(Arweave, Sia, etc.) would mean adopting a new dependency just to satisfy a
durability checkbox.

Per the principle: *each recursive call must increase independence*. Trading
one centralized capture (PL) for another (Arweave Foundation, etc.) does
not improve the position. The honest move is to acknowledge that "permanent
decentralized storage" is an aspiration, not an achievable single-step ship.

**What we have instead:**

- Source on GitHub (mirrorable to Codeberg / GitLab / sourcehut at any time)
- Static site small enough to fit in a single email attachment
- Cloudflare Pages serving from $0/mo
- DNS at Cloudflare, transferable in ~24 hours if needed
- Email routing at Cloudflare, replaceable in ~10 min

Any single layer above can be lost without losing the others. That is the
sovereign-recursion property in practice — not "no one can block you," but
"no single block can take you down."

**If durability becomes a real concern later:** push mirrors to Codeberg
+ GitLab + sourcehut, run a small VPS-hosted instance as a backup origin,
or revisit Arweave with a fresh wallet (no PL identity entanglement).

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

## What shipped (2026-05-07)

- [x] Source pushed to github.com/pondata/sovereignrecursion (public)
- [x] Cloudflare Pages connected, auto-deploys on push
- [x] `sovereignrecursion.com` resolves, HTTPS active
- [x] `justin@sovereignrecursion.com` forwards via Cloudflare Email Routing
- [x] Anchor essay live at `/essays/sovereign-recursion-jurisdictional-firewall/`
- [x] /essays/ index, /architecture/ stub
- [~] IPFS pin — **deliberately skipped**, see Step 4 above
- [ ] Essays #2 (L1–L5) and #3 (Eval Is the Work) — drafted in Codex Vivus
      monetization folder, awaiting publish
- [ ] HN submission of anchor essay
- [ ] recursive-agent-stack repo (separate from this one) — planned

The site is live. The principle is named in public.

## The next moves (when ready)

1. **HN submission.** Title: *Sovereign Recursion: Why Your Self-Improving
   Agent Needs a Jurisdictional Firewall.* URL:
   `https://sovereignrecursion.com/essays/sovereign-recursion-jurisdictional-firewall/`
   Best slot: Tuesday-Thursday morning Pacific. Comment substantively in
   your own thread for the first hour.

2. **Publish essay #2** (L1–L5: A Layered Model for Recursive Agents) at
   `/essays/l1-l5-layered-model-recursive-agents/`. Source markdown is at
   `~/codex-vivus/monetization/launch/essay-2-full.md`. Same conversion
   pattern as the anchor.

3. **Publish essay #3** (Eval Is the Work). Same pattern.

4. **Set up the open-source `recursive-agent-stack` repo** (separate from
   this site repo) — even an empty README + module structure is enough to
   make the link in the colophon real.

5. **Architecture page expansion.** Right now it's a stub. When the engagement
   patterns and pricing crystallize, this is where they live.
