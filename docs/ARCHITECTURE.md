# Architecture

## Trust zones

| Zone | Typical assets | Default posture |
|------|----------------|-----------------|
| **Gaming floor** | EGMs, TITO kiosks, table displays, pit tablets | Isolate from internet; allow only cage/TITO/account APIs on known ports |
| **Cage / count / vault** | Cage PCs, bill counters, vault terminals | Dual-control paths; no guest Wi-Fi; strict identity |
| **Surveillance / SOC** | CCTV, VMS, fraud dashboards | One-way ingest from floor; no browse-from-camera VLAN |
| **Back of house** | Finance, HR, marketing, Wi-Fi staff | Entra ID SAML; role-based web filter |
| **Online gaming DC** | RGS, wallet, KYC, payments, OTP | Separate from land floor; no hairpin through property guest Wi-Fi |
| **Guest / hotel** | Guest SSID, kiosks | Completely off gaming and cage networks |

## Identity

- **Entra ID (Azure AD) SAML** on FortiGate for staff, cage, surveillance, and IT.
- Groups map 1:1 to firewall policy packages (see [POLICY-MATRIX.md](POLICY-MATRIX.md)).
- Privileged roles (cage supervisor, vault, IT admin) require MFA in Entra ID.

## Connectivity

- **Site-to-site IPsec** between each property and corporate HQ / online DC.
- **SD-WAN** for dual carriers on the gaming and cage circuits.
- **No split-tunnel** for cage, vault, or surveillance VPN users.

## Logging

- Forward FortiGate logs to SIEM (or the fraud/anomaly dashboards).
- Retain per house policy and regulator; typical floor is 365–2555 days depending on data class.
