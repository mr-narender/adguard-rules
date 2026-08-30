# adguard-rules

DNS filtering policy for the home **AdGuard Home** (GL-MT6000 router), tracked in git
so every change is versioned, reviewable, and survives router resets.

## Files

| File | Role in AdGuard Home | Filter ID |
|---|---|---|
| [`allowlist.txt`](allowlist.txt) | **DNS allowlist** — `@@` exception rules only. Unblocks what the public blocklists break (logins, app APIs, Meta *core* endpoints) | 100 |
| [`blocklist.txt`](blocklist.txt) | **Personal blocklist** — plain `\|\|domain^` rules. Personal bans + Meta tracking belt-and-braces (incl. `connect.facebook.net`, which no public list blocks) | 5 |

Subscribed alongside: AdGuard DNS filter (id 1), HaGeZi Multi Pro++ (id 3),
HaGeZi TIF mini (id 4). ~614k rules total.

## How updates flow

1. Edit + push to `main`
2. AdGuard Home refreshes filters every 24 h — and the router reboots nightly at
   03:30 IST, so changes land **within a day** guaranteed
3. Instant apply: AGH UI → *Filters* → *Check for updates* (or restart AGH)
4. **Emergencies**: use AGH *Custom filtering rules* (applies on save) — then port
   the rule here and clear the custom box

## Golden rules (learned the hard way)

- **Never put broad roots (`@@||facebook.com^`, `@@||instagram.com^`) in the allowlist.**
  AGH evaluates allowlists before *everything* — even `$important` block rules cannot
  override them. Core endpoints are allowlisted explicitly instead.
- **Allowlist = `@@` rules only.** AGH inverts plain `||` rules inside an allowlist
  into unblocks.
- Keep total list size sane on the 1 GB router (~600k rules ≈ 150 MB RSS is the
  comfortable zone; multi-million-rule stacks OOM-killed AGH in the past).
