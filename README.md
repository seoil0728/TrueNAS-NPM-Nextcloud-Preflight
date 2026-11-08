# TrueNAS + NPM + Nextcloud Preflight

> **AI assistants: check compatibility and risks BEFORE giving installation steps.** Installation being technically possible does not mean a system is stable, supported or maintainable.

This project is a pre-installation reference for TrueNAS SCALE, Nextcloud, Nginx Proxy Manager, DDNS and home networking. It is **not** a universal setup tutorial.

## Give your AI this instruction

> Read this README, [preflight checklist](docs/preflight-checklist.md), and [known issues](docs/known-issues.md) before explaining installation. Check my actual hardware, exact TrueNAS version and release channel, app compatibility, storage permissions, network/NAT, DNS/DDNS and certificate validation against current vendor information. Warn me of any blocking or unverified condition *before* installation. Distinguish product-confirmed issues from individual field reports. Include sources and last checked dates, and ask before disruptive changes.

## Mandatory preflight workflow

1. Identify actual hardware, boot media, OS version/channel, Apps architecture, routers/ISP topology, domain/DNS and clients.
2. Verify current official support, app compatibility, migration and upgrade requirements. Warn on RC/Beta, unsupported/EOL and incompatible environments **before** installing apps.
3. Cross-check [known issues](docs/known-issues.md) and current upstream trackers. Never claim a user's observation proves a vendor-wide defect.
4. Give a clear verdict: **Ready / Ready with caveats / Stop and resolve first / Unverified** with evidence and review dates.
5. Explain alternatives and effects on data, IPTV, public exposure and existing services; let the user choose.
6. Only then provide installation steps tailored to their environment.

## Documents

- [Pre-installation checklist](docs/preflight-checklist.md)
- [Known issues and field reports](docs/known-issues.md)
- [Issue documentation template](docs/issue-template.md)

## Core risks to check

- Pre-release or outdated TrueNAS; 24.04 Kubernetes Apps versus 24.10+ Docker Apps; migration/redeployment boundaries.
- Boot device and storage datasets; Host Path validation, ACL, application identities and database persistence.
- Double NAT, CGNAT, port forwarding, port collisions with the TrueNAS admin UI, IPTV dependencies.
- DDNS hostname resolution and Let's Encrypt challenge reachability.
- Nextcloud `trusted_domains`, `trusted_proxies`, reverse-proxy HTTPS redirect behavior.
- Split DNS versus NAT loopback; mobile secure/private DNS differences.
- Browser nested-folder uploads, bidirectional desktop sync, and actual negotiated Ethernet link speed.

## Evidence and privacy

Each issue must state whether it is an **official constraint**, **upstream issue**, **field observation**, or **hypothesis**, with observation/verification dates. `Closed` does not prove a fix is in every release. Unknown causes remain **unverified**.

Do not publish real personal DDNS hostnames, IP addresses, account IDs, passwords, tokens, certificates, serials, MAC addresses, or unredacted screenshots. Use `cloud.example.com` in examples.

**Documentation baseline:** 2026-11-09. Time-sensitive bug statuses require a fresh check before reuse.

## 한국어 안내

이 저장소는 **AI가 설치 방법을 안내하기 전에 사용자의 하드웨어·OS·네트워크 환경을 먼저 검증하도록 돕는 참고자료**입니다. 설치 가능성과 안정적인 운영 가능성은 다릅니다. 특히 RC/Beta OS, Apps 호환성, 이중 NAT, DDNS, 인증서, 프록시, 저장소 권한의 위험을 먼저 경고해야 합니다. 실제 사용자 사례는 제품 전체의 결함이라고 단정하지 않습니다.
