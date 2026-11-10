# Pre-installation checklist / 설치 전 환경 점검

**Baseline: 2026-11-09.** This checklist is for an AI and user **before** any TrueNAS SCALE + Nextcloud + Nginx Proxy Manager installation instructions. Version-sensitive details must be checked again against current official sources.

## 1. Collect facts first

| Category | Required checks |
| --- | --- |
| Hardware | Actual model, architecture, RAM, NIC support, firmware and boot media |
| TrueNAS | Exact edition/build, stable vs RC/Beta, upgrade/migration path, support lifecycle |
| Storage | Boot/data separation, pool health, backups, datasets, ACLs, snapshots |
| Applications | Exact Nextcloud/NPM/PostgreSQL versions, chart/container support, persistent paths |
| Network | ISP router, own router, double NAT or CGNAT, IPTV and inbound port policy |
| HTTPS | Domain/DDNS, publicly resolvable A/AAAA, certificate challenge method, 80/443 listeners |
| Clients | LAN/WAN usage, Wi-Fi, mobile platforms, web upload and desktop synchronization |

**Stop and warn** if OS support status is unverified, an upgrade risks app redeployment, boot/pool health is unclear, there is no viable data backup, the public DNS or challenge path is unusable, or a proposed setting could expose an administrative service.

## 2. TrueNAS OS and Apps

- [ ] Verify **stable production release** and current support status before installing apps. RC/Beta are not presumed appropriate for reliable family cloud use.
- [ ] Review the exact release notes and Apps architecture. TrueNAS 24.04 used Kubernetes Apps; 24.10 transitioned to Docker Apps.
- [ ] Check the vendor's [app migration deadline notice](https://www.truenas.com/blog/app-migration-deadline/) and supported upgrade procedure; old 24.04 systems may need application redeployment.
- [ ] Verify boot-media compatibility, free RAM, disk health and dataset persistence.
- [ ] Determine host path ownership, ACL, UID/GID and DB data locations from the **current** app chart, not a previous user's sample.
- [ ] Never delete existing directories simply to bypass `path contains existing data` validation.

## 3. Networking and DNS

- [ ] Map Internet → ISP gateway → home router → NPM → Nextcloud.
- [ ] Identify **double NAT** vs **CGNAT**. Two local port-forward rules may resolve double NAT but cannot alone bypass ISP CGNAT.
- [ ] Consider IPTV/telephone dependencies before bridge mode, DMZ or router replacement.
- [ ] Make the NAS address stable; check port 80/443 conflicts against the TrueNAS management UI.
- [ ] Verify DDNS claim/renewal and public DNS A/AAAA answers; never assume the router's “DDNS connected” label guarantees external resolution.
- [ ] Select HTTP-01 or DNS-01 deliberately: [Let's Encrypt challenge types](https://letsencrypt.org/docs/challenge-types/). HTTP-01 requires a reachable external TCP port 80.
- [ ] Confirm ISP firewall or filtering and certificate renewal feasibility.

## 4. Reverse proxy and HTTPS

- [ ] Determine TLS termination and NPM upstream protocol/host/port.
- [ ] Confirm Nextcloud `trusted_domains`, accurate `trusted_proxies`, and appropriate protocol/host override settings where needed. [Nextcloud reverse proxy documentation](https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/reverse_proxy_configuration.html).
- [ ] Test login redirect chain, share URLs, WebDAV, uploads/downloads, not just the landing page.
- [ ] Ensure no administrative interface or database port is publicly exposed.
- [ ] Check certificate validity and renewal independently.

## 5. Same hostname on LAN and WAN

- [ ] Decide whether NAT loopback is supported or local split DNS is required.
- [ ] Verify actual DNS answers from each client, including secure/private DNS bypass possibilities.
- [ ] Test Windows/macOS, Android and iOS from LAN Wi-Fi and external data.
- [ ] Distinguish DNS errors from TCP refusal/timeout and TLS warnings. `ERR_CONNECTION_REFUSED` alone is not proof of DNS failure.

## 6. Data safety and synchronization

- [ ] Confirm nested browser folder uploads with a test tree and file counts.
- [ ] Explain desktop client's **bidirectional sync**: remote files may download, local files may upload, and deletion can propagate both ways.
- [ ] Check pre-existing folder conflict risks, filters, Virtual Files and sync error list.

## 7. AI report before installation

Provide:

1. **Verdict** — Ready / Ready with caveats / Stop and resolve first / Unverified.
2. **Exact blockers and cautionary conditions** — with product versions and applicability.
3. **Evidence quality** — official docs / upstream tracked issue / observed field report / hypothesis.
4. **Date checked and source link** — especially for claims such as “still unresolved”.
5. **Safer options and impact** — on backups, current services, network, IPTV and exposure.
6. **User decision** — get approval before disruptive upgrades, bridge/DMZ, permission wipes, or app redeployment.

Do not begin version-specific installation steps until the report has been presented.

## 8. Post-install acceptance tests

- [ ] HTTPS and login work with the intended hostname from WAN and LAN.
- [ ] Redirects stay HTTPS and certificate renewal path is viable.
- [ ] WebDAV, mobile login, sync, uploads, downloads and sharing permissions work.
- [ ] Nested folder counts and files match.
- [ ] Backup/restore plan exists; restarting services and routers does not unexpectedly break access.
