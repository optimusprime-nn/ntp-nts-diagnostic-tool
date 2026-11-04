# Testing an NTS Server

Enter the server's certificate-valid hostname, tick **Use NTS**, leave TLS verification on, and click **Start**.

The key exchange (NTS-KE) runs over TLS 1.3 on TCP 4460, unless you give another port as `host:port`:

```text
NTS-KE: connecting to time.cloudflare.com:4460; TLS certificate verification=ON
NTS-KE SUCCESS: TLSv1.3, TLS13_AES_256_GCM_SHA384, ALPN=ntske/1, AEAD=15 (AEAD_AES_SIV_CMAC_256)
NTS-KE: received 8 cookie(s) of 96 bytes; NTP endpoint=time.cloudflare.com:123
NTS-KE: C2S/S2C keys derived with the TLS exporter (key bytes are intentionally not logged).
```

The server's negotiation records choose the NTP endpoint. Each request then carries a fresh unique identifier and a cookie, and is authenticated with AES-SIV:

```text
Sending NTS request to time.cloudflare.com:123 (162.159.200.1:123): 224 bytes, UID 32 bytes, 0 cookie placeholder(s), AEAD_AES_SIV_CMAC_256
Received 224 bytes from 162.159.200.1:123
NTS AUTHENTICATED: UID matched, S2C tag verified, new cookies=1, pool=8
```

**UID matched** ties the reply to its request. **S2C tag verified** proves the server holds the keys from the key exchange. **new cookies** and **pool** show the cookie supply. When cookies run low the tool asks for more with placeholders, and when they run out it repeats the key exchange.

An **NTSN** Kiss-o'-Death means the server could not decrypt the cookie, for example after a key rotation. The tool discards its cookies and repeats the key exchange.

From the command line:

```text
ntpnts query time.cloudflare.com --nts -n 10 -d 500
ntpnts query ntp.lab.example:4460 --nts            # lab server on its own CA
```

## Rate limits

Many NTS servers rate-limit each client. Over the limit, a server may answer plain NTP with a Kiss-o'-Death RATE but silently drop NTS requests; RFC 8915 allows both. If you see loss only at short delays, try a delay of 1 s or more. One ESP32-P4 NTS server tested with 3.x let a burst of 8 through, then about one request every 3 s. Every reply it did send authenticated.

For certificate problems, see [Troubleshooting](Troubleshooting#certificates).
