# Testing an NTP Server

1. Enter the server's hostname or address, with `:port` if it isn't 123.
2. Untick **Use NTS**.
3. Choose **Requests** and **Delay**, and optionally **Log to file**.
4. Click **Start**.

Each reply is logged with its header fields and timing:

```text
NTP: LI=0 VN=4 Mode=4 Stratum=2 Poll=0 Precision=2^-23 RefID=... RootDelay=... RootDisp=...
Timing: offset=-4.818 ms | network delay=18.844 ms | server processing=0.077 ms
```

The client checks every reply: it must come from the server's address, echo the request's transmit timestamp as its originate timestamp, and be a version 4 server-mode packet. Replies that fail are counted as failures and say why.

Stratum 0 is a **Kiss-o'-Death**. The tool logs its code (RATE, DENY, RSTR and so on) and counts it under **Kiss-o'-Death**. It also warns about LI=3 (clock not synchronised), stratum 16, and unusually large root dispersion.

From the command line:

```text
ntpnts query pool.ntp.org -n 5
```

## Testing the other direction

To test an NTP *client* or *peer*, point it at the tool's responder instead. See [Peer Test Responder](Peer-Test-Responder).
