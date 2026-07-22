# Web Traffic Analysis: Plaintext vs. Encrypted (Wireshark)

**Author:** Alvin Espinoza **Date:** 07/21/2026 **Focus Area:** SOC Detection

---

## 1. Objective

I wanted to simulate a simple network sniffing exercise that SOC analysts work with everyday. I wanted to prove how much a SOC analyst can read encrypted vs unencrypted data when it comes to ports 443 and 80. I captured traffic that was running HTTP and also captured traffic to an HTTPS website. Almost all websites have HTTPS now so it was interesting to see data on a website with no TLS encryptions.

## 2. Environment & Tools

| Role                | Machine      | Address                         |
| ------------------- | ------------ | ------------------------------- |
| Analyst Wireshark | macOS laptop | 192.X.X.X (IPv4)                |
| Analyst (IPv6)      | macOS laptop | 2xxxxxxxxxxxxxx4:314c:883 |
| Local router / DNS  | Home gateway | 192.X.X.X                       |

**Tools used:**

- **Wireshark** - live packet capture and protocol analysis (interface `en0`)
- **Web browser** Safari - I generated the traffic by loading each site started clicking around.
- **Display filters** - `http`, `dns`, `tcp.flags.syn == 1`, `tls.handshake.type == 1`, `frame contains "..."`
- **Follow HTTP Stream** - to reassemble the plaintext conversation

**Targets:**

- **Plaintext:** `httpforever.com` — a site intentionally served over plain HTTP
- **Encrypted:** `krebsonsecurity.com` — a normal HTTPS site

**Capture files:**

- Plaintext: 13,500 packets
- Encrypted:  5,457 packets
