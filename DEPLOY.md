# Sovereign Recursion — deployment notes

## Files

```
~/sovereign-recursion/
├── index.html                                  ← homepage
├── case-studies/
│   └── codex-vivus.html                        ← reference-implementation case study
├── sales/
│   ├── target_orgs.md                          ← internal target list (gitignore if repo'd)
│   └── letters/
│       ├── 01_pnnl.md                          ← internal — drafts, not for publishing
│       ├── 02_madrona.md
│       └── 03_stoel_rives.md
├── assets/                                     ← (empty for now; for any logos/images later)
└── DEPLOY.md                                   ← this file
```

## Hosting (cheapest, fastest)

**Cloudflare Pages** — recommended.

1. Push `~/sovereign-recursion/` to a private GitHub repo (e.g. `sovereign-recursion`)
   - **Important:** add `sales/` to `.gitignore` if making the repo public; the letter drafts and target list are internal
2. Connect the repo to Cloudflare Pages
3. Build settings: none needed — pure static site
4. Custom domain: `sovereignrecursion.com` (DNS already at Cloudflare per the domain list)
5. SSL is automatic

Total cost: $0/month (Cloudflare Pages free tier covers far more than this site will ever need).

## Email setup

`justin@sovereignrecursion.com` referenced in the case study + letters needs to be live before letters go out.

**Cloudflare Email Routing** (free): forward `justin@sovereignrecursion.com` → your existing inbox. Ten minutes to set up at dash.cloudflare.com → Email → Email Routing.

If you want to *send* from `justin@sovereignrecursion.com` (recommended for letter follow-ups), you'll need either:
- Google Workspace ($7/mo per user) — full email account, send + receive
- Or set up the domain in your existing email provider (e.g. add as alias in Fastmail / iCloud+ / etc)

## Suggested .gitignore for the repo

```
sales/
.DS_Store
*.swp
```

## Pre-launch checklist

- [ ] Push site to Cloudflare Pages
- [ ] Verify `sovereignrecursion.com` resolves
- [ ] Verify `https://sovereignrecursion.com/case-studies/codex-vivus.html` loads
- [ ] Set up `justin@sovereignrecursion.com` email routing
- [ ] Personalize each of the three letters (CIO/Managing Partner names)
- [ ] Verify physical mailing addresses are current
- [ ] Print + sign + mail letters (cost: ~$3 per letter postage)
- [ ] Schedule the email follow-ups for week 2

## What this site does NOT do (yet)

- No analytics. Cleaner brand. Add later if needed.
- No blog. The case study is enough surface for first 6 months.
- No "About" page. The case study + homepage carry the bio. Add only if asked for.
- No testimonials. After first engagement closes, add a single quote on the homepage.
- No pricing calculator. Pricing is in the engagement-pattern blocks; specific quotes happen in the diagnostic call.
- No newsletter / lead magnet. Wrong shape for this audience.

The site stays minimalist on purpose. The buyer is reading 5,000 words of case study, not browsing a marketing funnel.
