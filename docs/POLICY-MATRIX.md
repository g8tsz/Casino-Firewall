# Role-based policy matrix

Map Entra ID groups to FortiGate policy packages. Adjust ports and destinations per property.

| Entra ID group | Allowed destinations | Web filter | Notes |
|----------------|----------------------|------------|--------|
| `casino-cage` | Cage app, TITO API, account system, printers | Block social, cloud storage, personal mail | No internet except payment/KYC allow-list |
| `casino-count-vault` | Count/vault apps, armored-car vendor VPN | Same as cage | Dual-control jump hosts only |
| `casino-surveillance` | VMS, fraud tracker, anomaly dashboard | Limited | Camera VLAN is destination-only |
| `casino-slot-tech` | EGM vendor portals, manufacturer support | Allow-list | No guest SSID |
| `casino-pit` | Table systems, player tracking | Tight allow-list | |
| `casino-marketing` | CRM, ads, analytics | Standard corporate | No floor or cage |
| `casino-it-admin` | FortiManager, Entra, jump hosts | Full with MFA | Change windows only |
| `casino-guest` | Guest internet | Captive portal | No overlap with RFC1918 gaming ranges |
| `casino-online-ops` | RGS, wallet, KYC, OTP, payment adapters | Allow-list SaaS | Land floor routes blocked |

Do not put production API keys or FortiGate admin passwords in this repo. Use FortiManager / Entra secrets and local `.env` files that stay gitignored.
