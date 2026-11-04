# Stability Testing

For a long run, raise **Requests**, choose a **Delay**, tick **Log to file**, choose a file, and **Start**. For example, 500 requests at 5000 ms runs for about 42 minutes.

Watch the counters and the **Timing** charts during the run. At the end, check the **SUMMARY** lines:

- **Received** should equal **Sent**, and with NTS so should **Authenticated**.
- **NTS-KE exchanges** should stay at 1, unless the server rotates its keys (an NTSN Kiss-o'-Death) or the cookies run out.
- Offset **stddev** and **jitter** show how steady the path and server are.

From the command line the same run is:

```text
ntpnts query time.example --nts -n 500 -d 5000 --log stability.log
```

`ntpnts` exits with code 1 if any request failed, so it can gate a CI job or a scheduled task.

Pick a delay the server's rate limit allows. A delay under a second can trigger rate limiting that looks like packet loss; see [Testing an NTS Server](Testing-an-NTS-Server#rate-limits).
