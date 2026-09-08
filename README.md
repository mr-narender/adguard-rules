# adguard-rules

DNS filtering policy for the home **AdGuard Home** (GL-MT6000 router), tracked in git
so every change is versioned, reviewable, and survives router resets.

## Files

| File | Role in AdGuard Home | Filter ID |
|---|---|---|
| [`allowlist.txt`](allowlist.txt) | **DNS allowlist** — `@@` exception rules only. Unblocks what the public blocklists break (logins, app APIs, Meta *core* endpoints) | 100 |
| [`blocklist.txt`](blocklist.txt) | **Personal blocklist** — plain `\|\|domain^` rules. Personal bans + Meta tracking belt-and-braces (incl. `connect.facebook.net`, which no public list blocks) | 5 |

Shared personal rules are subscribed alongside hardware-sized community profiles:

- **OPNsense (32 GB):** HaGeZi Ultimate + full TIF + Dandelion Sprout Anti-Malware.
- **GL-MT6000 (1 GB):** HaGeZi Pro++ Mini + TIF Mini.

Overlapping aggregate lists are intentionally avoided; each router uses one HaGeZi
Multi tier plus TIF and the personal block/allow lists.

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
- Keep total list size sane on the 1 GB router. Pro++ Mini + TIF Mini is about
  238k rules and 75–80 MB RSS; multi-million-rule stacks OOM-killed AGH in the past.
