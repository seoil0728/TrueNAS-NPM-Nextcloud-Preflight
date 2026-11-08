# Known issues and field observations

**Document baseline:** 2026-11-09 (Asia/Seoul). **Purpose:** prevent risky installs by making an AI identify known constraints and observed failure modes **before** deployment. The date is not a claim that all upstream issue states were freshly verified.

## Evidence and status conventions

- **OFFICIAL CONSTRAINT** — Vendor documented support or architecture restriction, not necessarily a software bug.
- **UPSTREAM ISSUE** — Linked GitHub issue/PR. Check affected and fixed **release versions**, not only Open/Closed.
- **FIELD REPORT** — Observed during one TrueNAS + NPM + Nextcloud deployment. A single observation does **not** establish a product-wide defect.
- **HYPOTHESIS** — Plausible but not proven cause.
- **Operationally resolved** — A working setup or workaround was reached; does not necessarily explain the root cause.
- **Last confirmed observation** means the last date for this deployment, not an external tracking site's current status.

## Overview

| ID | Topic | Type | Field status |
| --- | --- | --- | --- |
| TN-01 | TrueNAS pre-release and 24.04/24.10 Apps migration | OFFICIAL CONSTRAINT + FIELD REPORT | Upgraded/redeployed |
| TN-02 | eMMC boot partition error | FIELD REPORT | Later booted; root cause unknown |
| TN-03 | Existing Host Path data, ACL, app UID/GID, DB paths | FIELD REPORT | Reconfigured |
| NET-01 | ISP gateway plus home router: double NAT and IPTV | FIELD REPORT | Two-hop port forwarding worked |
| NET-02 | TrueNAS admin vs NPM 80/443 port collision | FIELD REPORT | Ports separated |
| DNS-01 | TP-Link DDNS availability and Let's Encrypt NXDOMAIN | FIELD REPORT | Alternate DDNS worked |
| DNS-02 | Same HTTPS hostname within LAN, split DNS | FIELD REPORT | Configured |
| HTTPS-01 | Untrusted domain and HTTPS → HTTP login redirect | FIELD REPORT | Configured |
| MOB-01 | Android private DNS differences | FIELD REPORT | Root cause unverified |
| MOB-02 | iOS browser worked, Nextcloud app login failed | FIELD REPORT | Later worked; cause unknown |
| MOB-03 | Intermittent Wi-Fi ERR_CONNECTION_REFUSED | FIELD REPORT | Self-recovered |
| FILE-01 | Folder-upload menu skipped nested files | FIELD REPORT | No verified fix |
| FILE-02 | Nextcloud folder drag-and-drop issue | UPSTREAM ISSUE | Tracker closed; verify deployed release |
| LAN-01 | PC 1Gbps/100Mbps link instability | FIELD REPORT | Recurring, cause unknown |

---

## TN-01 — TrueNAS release channel and Apps migration

**Observed:** 2026-11-04 to 2026-11-06. Started from TrueNAS SCALE `24.04-RC.1`, then updated through newer releases. Apps required redeployment while the storage pool/datasets remained available.

**Confirmed architecture constraint:** 24.04 Apps were Kubernetes based and 24.10 introduced Docker-based Apps. TrueNAS has documented a **June 1, 2025** deadline for automatic 24.04-to-24.10 app migration support. [Vendor announcement](https://www.truenas.com/blog/app-migration-deadline/) · [24.04 Apps documentation](https://www.truenas.com/docs/scale/24.04/scaleuireference/apps/).

**AI preflight warning:** Determine exact build and release status before installing Nextcloud/NPM. Flag RC/Beta/old releases and migration/redeployment hazards first; choose a supported stable upgrade path before adding apps.

**Status:** Official architecture/migration constraint; apps eventually installed. It is **not proven** that every observed installation problem was caused by the RC build.

## TN-02 — eMMC/boot-media error

**Observed:** early November 2026. `Failed to find partition number 2 on mmcblk0` was reported during boot/device work. Subsequent OS startup succeeded.

**AI warning:** Validate boot medium, firmware, partition handling, hardware compatibility and recovery route. Do not generalize one mini-PC's symptom to every eMMC device.

**Status:** Cause unknown; no confirmed hardware product defect.

## TN-03 — Host paths, ACL and persistent data

**Observed:** 2026-11-04 to 2026-11-06. Host Path validation produced `path contains existing data and 'force' was not specified`; datasets for Nextcloud user/app files, PostgreSQL and NPM certificates/config required permission work.

**AI warning:** Check pre-existing data, exact chart storage fields, app process UID/GID, ACL, backup and restore plan before changing permissions. Avoid universally copying a numeric UID or wiping a populated directory.

**Status:** Operationally configured; not established as an upstream bug.

## NET-01 — Double NAT and IPTV implications

**Observed:** 2026-11-04 to 2026-11-06. ISP router → private router → NAS meant that forwarding only at the inner router was insufficient. Port forwards at both layers enabled external HTTPS; bridge mode was avoided due to IPTV implications.

**AI warning:** Identify double NAT **and** possible CGNAT before promising external port access. Two-hop port forwarding does not solve ISP CGNAT. Discuss IPTV dependencies, bridge, DMZ and alternative remote-access architectures.

**Status:** Working in observed topology; double NAT remains as architecture, not a software bug.

## NET-02 — HTTP(S) port overlap

**Observed:** early November 2026. TrueNAS UI and NPM public HTTP(S) listener could conflict or receive the wrong redirected request.

**AI warning:** Map binding IPs and ports before setup; keep TrueNAS admin private and allocate public 80/443 explicitly to intended proxy endpoints.

**Status:** Operationally resolved through distinct service ports.

## DNS-01 — TP-Link DDNS and certificate issuance

**Observed:** 2026-11-04 to 2026-11-06. Attempted TP-Link DDNS registration/hostname choice encountered issues. During certificate issuance, Let's Encrypt returned `NXDOMAIN looking up A` and `NXDOMAIN looking up AAAA`. Changing to another DDNS provider preceded successful validation.

**AI warning:** Verify hostname registration, public A/AAAA responses, address family, propagation and actual external challenge routing *before* trying certificates. `NXDOMAIN` indicates resolution failure at that time and **does not prove** a general TP-Link firmware/DDNS defect.

**Status:** Operationally resolved with alternate DDNS provider; original root cause not verified. [Let's Encrypt challenge types](https://letsencrypt.org/docs/challenge-types/).

## DNS-02 — Internal access using public hostname

**Observed:** early November 2026. Internal Wi-Fi and external connectivity differed with the same HTTPS hostname. A local resolver and split DNS rewrite were configured to return the internal proxy address.

**AI warning:** Identify router NAT loopback/hairpin support and check which DNS server clients actually use; test on each internal/external network.

**Status:** Working after split DNS, but a later transient connection failure was separately observed (MOB-03).

## HTTPS-01 — Reverse proxy headers, domain trust and redirects

**Observed:** early November 2026. Nextcloud displayed an untrusted-domain warning and at one point returned an HTTP login redirect for an HTTPS request, potentially reaching the wrong service.

**AI warning:** Check actual NPM upstream, Nextcloud `trusted_domains`, `trusted_proxies`, forwarded scheme and correct `overwriteprotocol` where relevant. Never copy proxy network addresses from another deployment. Validate the complete redirect chain. [Nextcloud admin docs](https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/reverse_proxy_configuration.html).

**Status:** Operationally resolved through configuration; not a confirmed upstream defect.

## MOB-01 — Android private DNS correlation

**Observed:** early November 2026. Home Wi-Fi plus Android Private DNS = Automatic coincided with `ERR_CONNECTION_REFUSED`; turning it off worked. Outside hotspot plus Automatic worked. An iOS browser also worked over the home Wi-Fi.

**AI warning:** Capture DNS answers and TCP route at failure time, not merely the setting label.

**Status:** Root cause unverified. Do **not** claim Android Private DNS definitively caused the issue.

## MOB-02 — iOS app login vs Safari

**Observed:** early November 2026. Safari HTTPS worked while the Nextcloud iOS app could not log in. A later login succeeded on external data; the same app then worked on local Wi-Fi.

**AI warning:** Treat app authorization/login flow separately from browser HTTPS connectivity. Do not present “log in on external data first” as a proven fix.

**Status:** Functionally resolved in the observed environment; causal mechanism unknown.

## MOB-03 — Temporary Wi-Fi connection refused

**Observed:** 2026-11-07 to 2026-11-08. `ERR_CONNECTION_REFUSED` accessing the HTTPS hostname from home Wi-Fi; access later returned without a documented configuration change.

**AI warning:** Check actual DNS answer, resolved IP, port 443 and proxy health **during** failure. Refusal does not alone demonstrate the split-DNS resolver stopped.

**Status:** Recovered; root cause unverified.

## FILE-01 — Nested-folder upload via menu

**Observed:** 2026-11-08. Files in nested subfolders were missed when the Nextcloud web UI's **folder upload menu** was used.

**AI warning:** Verify the resulting file tree/counts; distinguish menu picker from drag-and-drop bugs. Desktop sync supports recursive trees but is **bidirectional**, and its exclusion/error list needs checking.

**Status as observed 2026-11-09:** Affected that setup; exact app version/root cause/upstream fix unverified. **Not matched** to FILE-02.

## FILE-02 — Public Nextcloud drag-and-drop issue

**Upstream:** [nextcloud/server #58454](https://github.com/nextcloud/server/issues/58454) and related [PR #59294](https://github.com/nextcloud/server/pull/59294). This report concerns **drag-and-drop** folder uploads where directories were created while nested files could be missing.

**Tracker status last discussed:** Closed/fix merged, but a closed ticket does not establish that every deployed minor/patch release contains the change. This record does **not** claim a new live check of a particular patch as of today.

**AI warning:** Check the precise Nextcloud patch version, browser, storage target, upload method and release/backport history. This issue is **not evidence** of the menu-path symptom in FILE-01.

## LAN-01 — 1Gbps to 100Mbps renegotiation and link flap

**Observed:** 2026-11-08. A Windows PC with a 2.5GbE-capable Realtek NIC reported only **100 Mbps** and uploaded around **10 MB/s**. After a network interface restart it sometimes negotiated **1 Gbps** and uploads reached **112 MB/s**. It later disconnected intermittently and returned to 100 Mbps. Cable reseating and router LAN-port switching did not conclusively resolve it.

**AI warning:** Verify **actual negotiated link speed** on both ends, cable terminations, port contact, adapter drivers, power-saving/EEE and a known-good replacement cable if practicable. A Cat8 marking is not proof of good cable condition.

**Status as observed 2026-11-09:** Recurrent, underlying fault unverified. No claim that a specific cable, motherboard or router is defective.

---

## Maintenance

Keep field observation dates separate from dates when public issue trackers are **actually revisited**. Never mark a bug “unresolved as of YYYY-MM-DD” solely because the original issue was once open. Add source links and exact affected versions when verifiable; separate a workaround from a code fix. Remove personal hostnames, live IPs, accounts, MACs, serials, secrets and raw logs before publishing.
