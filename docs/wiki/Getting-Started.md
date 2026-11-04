# Getting Started

1. Install the tool (see [Installing](Installing)) and start **NTP-NTS Diagnostic Tool** from the Start menu.
2. Windows asks nothing at this point. The tool may offer to restart as Administrator: choose **Continue**, since you only need that for the firewall. Tick **Don't ask at startup** to stop the question.
3. In the **Client** card, enter `time.cloudflare.com` as the **Server**.
4. Set **Requests** to `5` and **Delay** to `1000` ms.
5. Tick **Use NTS (RFC 8915, TCP 4460)**. Leave **Skip TLS certificate verification** off.
6. Click **Start**, or press **Ctrl+Enter**.

The log shows the key exchange, then one authenticated exchange per request:

```text
NTS-KE SUCCESS: TLSv1.3, TLS13_AES_256_GCM_SHA384, ALPN=ntske/1, AEAD=15 (AEAD_AES_SIV_CMAC_256)
NTS-KE: received 8 cookie(s) of 96 bytes; NTP endpoint=time.cloudflare.com:123
...
NTS AUTHENTICATED: UID matched, S2C tag verified, new cookies=1, pool=8
```

The counters along the top should read **Sent 5**, **Received 5** and **Authenticated 5**, with zeros elsewhere. The **Timing** card charts offset and delay for each reply.

For plain NTP, untick **Use NTS** and try `pool.ntp.org`.

Next: [The Window](The-Window).
