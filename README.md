# Casino Firewall

Identity-aware **FortiGate NGFW** design for **land-based properties** and **online casino** platforms. Staff, cage, surveillance, slot techs, and online ops authenticate through **Microsoft Entra ID (Azure AD) SAML**. Traffic is split into gaming-floor, cage/count/vault, surveillance, back-of-house, guest, and online-DC zones.

**Repository:** [github.com/g8tsz/Casino-Firewall](https://github.com/g8tsz/Casino-Firewall)

This is an architecture and policy pack, not a dump of live firewall configs.

## Who it is for

- **Land** — slot floor, pit, cage, count room, vault, surveillance, hotel/guest Wi-Fi
- **Online** — RGS, wallet/credits, KYC, OTP, prize redemption, payment adapters
- **Corporate** — multi-property HQ with site-to-site VPN to each casino and to the online DC

Related floor systems in this account (optional to pair): [Account-system](https://github.com/g8tsz/Account-system), [Marker-TITO-replacement](https://github.com/g8tsz/Marker-TITO-replacement), [casino-kyc-integration](https://github.com/g8tsz/casino-kyc-integration), [Casino-Fraud-Cheating-tracker](https://github.com/g8tsz/Casino-Fraud-Cheating-tracker).

## Problems this design addresses

- Many properties, each with its own FortiGate, and **no shared identity** for who may reach cage, TITO, or EGM vendor tools
- Guest Wi-Fi and hotel networks **bridging** into gaming or cashier VLANs
- Online wallet, KYC, and payment APIs reachable from the **wrong** network
- No **role-based web filter** (marketing vs cage vs IT)
- Weak visibility for **gaming commission / internal audit** (who accessed what, from where)

## Solution

- **FortiGate NGFWs** at each property and at the online DC
- **Entra ID SAML** for user and group-based access (cage, vault, surveillance, slot tech, pit, marketing, IT, guest)
- **Granular policies** per role; guest SSID never routes to RFC1918 gaming or cage ranges
- **Site-to-site IPsec** from each property to HQ and to the online DC (no hairpin of cage traffic over guest internet)
- **Standard policy packages** so a new property gets the same zones and groups

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) and [docs/POLICY-MATRIX.md](docs/POLICY-MATRIX.md).

## Outcomes

- Identity-based access: Entra groups decide who can hit cage, vault, and online ops
- Same zone model on every property and in the online DC
- Role-based web filtering for staff; allow-list only for cage and online payments
- Logs suitable for SIEM and fraud/surveillance tooling
- Repeatable cutover: clone policy package, bind local interfaces, join Entra

## Rollout outline

1. Draw zones (floor, cage/count/vault, surveillance, BOH, guest, online DC).
2. Create Entra ID groups listed in the policy matrix; enforce MFA on privileged groups.
3. Stand up FortiGate SAML (SP) against Entra; map groups to firewall users.
4. Apply policy packages; verify guest cannot ping cage or EGM VLANs.
5. Bring up site-to-site VPN; fail over SD-WAN if dual circuit.
6. Point logs at SIEM; cut over one property, then clone.

## What is not in this repo

- Live FortiGate backups, API keys, SAML certificates, or webhook URLs
- Claims of GLI, PCI DSS, or regulator certification — those are site assessments

## License

MIT. See [LICENSE](LICENSE).
