# Public NTP servers

> **Disclaimer.** These lists are published for defensive cybersecurity only:
> network defence, monitoring, detection and incident response. They must not
> be used for malicious purposes, such as attacking, scanning, abusing or
> overloading the listed servers, which are public services run by volunteers
> and vendors.

Daily updated lists of the IPv4 addresses of public time (NTP) servers from
the main providers, checked to answer the time, ready to load into a firewall,
a SIEM or any tool that accepts an IP list. They are meant to help SOC teams
tell legitimate time traffic apart from everything else on udp/123.

## Files

| File | Content |
|---|---|
| `ntp-all-v4.txt` | Every unique IP from every provider, one per line. **The list to load.** |
| `ntp-all-v4.csv` | One row per IP: providers and hostnames that list it, first and last seen, latest probe result. |
| `ntp-all.json` | Status of each provider's last update, and per-IP detail. |
| `providers/ntp-<provider>-v4.txt` | One provider only. |

Every per-provider `.txt` comes with a `.csv` and a `.json` of the same name
with the detail of its last update.

## Providers

| Provider | Hostnames | List |
|---|---|---|
| NTP Pool | `pool.ntp.org` and its country and continent zones | `providers/ntp-pool-v4.txt` |
| Ubuntu | `ntp.ubuntu.com` | `providers/ntp-ubuntu-v4.txt` |
| Microsoft | `time.windows.com` | `providers/ntp-microsoft-v4.txt` |
| Apple | `time.apple.com`, `time.euro.apple.com`, `time.asia.apple.com` | `providers/ntp-apple-v4.txt` |
| Google | `time.google.com`, `time1-4.google.com` | `providers/ntp-google-v4.txt` |
| Cloudflare | `time.cloudflare.com` | `providers/ntp-cloudflare-v4.txt` |
| Meta | `time.facebook.com`, `time1-5.facebook.com` | `providers/ntp-meta-v4.txt` |
| AWS | `time.aws.com` | `providers/ntp-aws-v4.txt` |

## Use cases

- **Outbound allowlist.** Allow udp/123 only towards `ntp-all-v4.txt` (or to
  the providers your fleet actually uses) instead of to the whole internet.
- **Detection of non-standard NTP.** Alert on udp/123 traffic to destinations
  that are not on the list: misconfigured devices, unmanaged hosts, or NTP used
  as a covert channel or for data exfiltration.
- **Log enrichment.** Load `ntp-all-v4.csv` as a SIEM lookup table to tag
  connections with the provider and hostname behind each IP.
- **Triage.** Answer quickly whether an IP seen in an alert is a known public
  time server and whose it is.
- **Asset hygiene.** Find hosts that sync against the internet when they
  should use the internal time servers.
- **Reflection and amplification triage.** Distinguish expected replies from
  public time servers from unsolicited NTP traffic hitting your network.

## How to use

1. Open the file and press **Raw**. The address bar shows its direct URL
   (`raw.githubusercontent.com/...`).
2. Point the firewall's external list (EDL, feed, dynamic address object) or
   the SIEM lookup at that URL.
3. Refresh once a day. The lists are updated daily and do not change faster.

Without external list support, download the `.txt` and load it by hand; it
works the same and has to be repeated now and then.

## Limits

- **The lists are not exhaustive.** Some providers hand out servers at random
  or depending on the region that asks. Coverage grows over the days but never
  reaches 100%: do not block critical time traffic only on these lists.
- **Listed does not mean trusted.** An IP here is a known public time server,
  not a safe destination for anything else.
- **Your ISP's time servers are not included.** Allow them separately if you
  use them.
- **An aid, not a control.** The strong control is architecture: a few
  internal time servers that are the only ones allowed to reach the internet.
