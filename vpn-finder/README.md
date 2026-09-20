# Commercial VPN provider IP lists

IPv4 and IPv6 addresses of commercial VPN servers, **attributed by provider**
and with the hostname that served them when it can be established. Regenerated
once a day and published here.

This repository holds the data only. The generator lives elsewhere.

## What these lists are

They are the addresses the providers themselves publish on their open
endpoints, collected and normalized. None of this is private or obtained by
bypassing authentication.

**What they are not:**

- Not a complete list of commercial VPN space. Whole providers are missing
  (see below) and no public inventory is up to the minute.
- Not datacenter or generic hosting addresses. That noise is left out on
  purpose.
- **Segment** attribution is probabilistic. A `/24` marked with a provider
  means every IP we have seen in that segment is theirs, not that the whole
  segment belongs to them. Never report it as confirmed.

## Sources

| Provider | Endpoint | IPv6 |
|---|---|---|
| NordVPN | `https://api.nordvpn.com/v1/servers?limit=0` | AAAA resolution |
| Mullvad | `https://api.mullvad.net/www/relays/all/` | published (`ipv6_addr_in`) |
| PIA | `https://serverlist.piaservers.net/vpninfo/servers/v6` | AAAA resolution |
| Surfshark | `https://api.surfshark.com/v3/server/clusters` | AAAA resolution |
| IPVanish | `https://configs.ipvanish.com/configs/configs.zip` | AAAA resolution |

All of them are public and unauthenticated.

### Providers not covered

- **ProtonVPN**: closed anonymous access to `api.protonvpn.ch/vpn/logicals`.
- **Windscribe**, **CyberGhost**, **ExpressVPN**: no public inventory endpoint.

Nothing is scraped, no login is bypassed and no credentials are used. Until an
open source exists, these providers are absent and the gap is declared here
instead of being papered over.

### About IPv6

Only Mullvad hands over IPv6 directly. For the rest it comes from resolving
AAAA over the hostnames, and most have no AAAA record. IPv6 coverage is
therefore much lower than IPv4: that is a fact of the ecosystem, not a failure.
Exact per-run counts are in `output/metadata.json`.

## Formats

| File | Contents |
|---|---|
| `output/ipv4.txt` | One IPv4 per line, numeric order. |
| `output/ipv6.txt` | One IPv6 per line, RFC 5952 compressed form, numeric order. |
| `output/cidr/ipv4.txt` | The same IPv4 addresses aggregated to the **exact** minimal CIDR. |
| `output/cidr/ipv6.txt` | Same for IPv6. |
| `output/by-provider/<provider>-ipv4.txt` | Only that provider's IPv4 addresses. |
| `output/by-provider/<provider>-ipv6.txt` | Same for IPv6. May be empty. |
| `output/attributed.csv` | `ip,familia,proveedor,hostname,primera_vez,ultima_vez` (column names are literal, as emitted). One row per IP/hostname pair. |
| `output/segments/ipv4-24.tsv` | `segment <TAB> provider`, or the literal `AMBIGUO` when more than one provider is seen. |
| `output/segments/ipv6-64.tsv` | Same for `/64`. |
| `output/deltas/YYYY-MM-DD.json` | Additions and removals for that run. Only created if there were any. |
| `output/metadata.json` | Counts, timestamps and per-source health. |
| `CHANGELOG.md` | Readable summary of every run that published changes. |

The `.txt` files carry no headers and no comments: they are meant to be loaded
straight into a firewall or a script.

CIDR aggregation never invents coverage. A block only appears if **all** of its
addresses are in the inventory; if the range is not exact, the addresses come
out individually as `/32` or `/128`.

## Cadence and stability

One run per day. The repository only receives a commit if the content really
changed: `metadata.json` carries timestamps but is not enough on its own to
produce a commit, so a `git diff` shows real changes and not formatting noise.

An IP that disappears from the inventory **is not deleted right away**: it is
kept for 30 days with its `ultima_vez`, and only then retired. VPN servers
rotate, and an IP retired yesterday is still useful evidence today. That
difference is in `metadata.json` as `active` versus `retained`.

If a source does not respond, its previous list is kept and marked `stale` in
`metadata.json`. A provider's empty list is never published. If the total were
to drop by more than 20% in one run, nothing is published: a jump like that is
almost always a broken source, not a real infrastructure change.

## Responsible use

These are IP addresses of servers of legal commercial services, published by
the providers themselves. They are useful for classifying traffic and applying
network policy.

**They are not useful for flagging people.** Using a VPN is not an indicator of
malicious activity: it is an ordinary privacy tool, and in many contexts it is
the recommended practice. Treating a match against these lists as a risk signal
about an individual is a misuse of the data.
