<p align="center">
  <img src="images/banner.jpg" alt="nexus" width="100%">
</p>

# nexus — External Dynamic Lists (EDL)

Daily-updated IPv4/IPv6 feeds in plain text, one entry per line, no headers.
Data only: the generator lives elsewhere.

## 1. Feeds

| Feed | Contents | Family | Updated |
|---|---|---|---|
| [`ntp-finder`](ntp-finder/) | Public time servers from the NTP pool, verified | IPv4 | Daily |
| [`vpn-finder`](vpn-finder/) | Commercial VPN provider infrastructure, attributed per provider | IPv4, IPv6 | Daily |

Each directory has its own README covering formats, sources and known limits.
Read it before using the list.

## 2. Usage

```
https://raw.githubusercontent.com/k0jir0900/nexus/main/<feed>/<file>.txt
```

```bash
curl -sO https://raw.githubusercontent.com/k0jir0900/nexus/main/ntp-finder/output/ntp-pool-v4.txt
```

## 3. Use cases

A SOC is rarely short of events. What it lacks is the context that tells an
analyst which ones deserve the next ten minutes. These feeds answer one
question about the traffic you already collect — what kind of infrastructure
sits on the other end — and each answers it for a different purpose:

| Feed | Role | In practice |
|---|---|---|
| `ntp-finder` | **Exclude** | Known-good time infrastructure. Filter it out and review what is left. |
| `vpn-finder` | **Observe** | Legitimate infrastructure with a context worth carrying. Label it, correlate it, do not alert on it alone. |

### 3.1 Ingestion

Ingest the plain lists — one address per line, no headers, nothing to parse:

| File | Family |
|---|---|
| `ntp-finder/output/ntp-pool-v4.txt` | IPv4 |
| `vpn-finder/output/ipv4.txt` | IPv4 |
| `vpn-finder/output/ipv6.txt` | IPv6 |

Any firewall that reads a remote dynamic list, any IDS that loads a dataset and
any SIEM that ingests a lookup can point at the raw URL directly and re-read it
on its own schedule. Daily is enough; nothing here changes faster. Where remote
fetch is not available, download the `.txt` and load it manually.

Confirm what your platform does when the fetch fails: keeping the last good
copy is the behaviour you want, silently emptying the list is not.

### 3.2 Detection

#### UC-01 — Unrecognised time destinations

UDP/123 egress to a destination outside the pool list and outside your vendor's
time service. Almost always configuration drift; occasionally a covert channel
riding a port nobody inspects. The feed is the exclusion set, so precision
depends on how complete it is — treat the remainder as a review queue, not an
alert.

#### UC-02 — Scanning and credential attacks from VPN infrastructure

Inbound probing, enumeration and password spraying carry more weight when the
source is attributed. Commodity scanning arrives from hosting and residential
space; an actor paying for a tunnel to reach your perimeter is making a choice.
Group perimeter and WAF events by provider and by first-seen date — rotation
across addresses of the same provider is the signal, not any single hit.

#### UC-03 — Authentication from VPN infrastructure

Never an alert on its own — commercial VPN use is ordinary privacy practice.
It earns its place as a correlation term: impossible travel, a first-seen
device, a failed-then-successful sequence. The provider and first-seen fields
in the CSV are what make the enrichment readable.

## 4. License

GPL-3.0 ([LICENSE](LICENSE)). A per-directory license, where present, governs
the files in that directory.