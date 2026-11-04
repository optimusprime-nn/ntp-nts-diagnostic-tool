# Windows Firewall and Administrator

The client never needs Administrator. Only the responder does: to let other machines reach it through Windows Firewall, and sometimes to bind UDP/123.

## At startup

As in 2.x, the tool checks its rights when it starts:

- **Not elevated:** it offers **Restart as Administrator** through the normal UAC prompt. **Continue** carries on as you are. If you decline the UAC prompt, the tool says so and carries on.
- **Elevated, without a firewall rule for the responder port:** it offers **Allow UDP/*port***. **Not now** carries on without one.

Tick **Don't ask at startup** in either prompt to stop them. The **Windows** card keeps the same buttons.

## The Windows card

The card shows:

- whether the tool is running as Administrator or as a standard user
- the firewall rule's state: missing, disabled, allowing the responder's port, or allowing a different port from the one the responder uses
- a warning when the Windows Time service (W32Time) is running and the responder is set to UDP/123

**Allow UDP/*port*** creates or refreshes the inbound rule **NTP-NTS Diagnostic Tool Peer Responder**. It allows UDP on the responder's current port, for the **Domain** and **Private** profiles only; Public networks are never opened. **Remove rule** deletes it. Both need Administrator.

The refresh button re-reads the status, for example after you change the rule in Windows Defender Firewall yourself.

## Windows Time and UDP/123

W32Time normally owns UDP/123, so the responder can't bind it. Either pick another port, or stop the service from an elevated prompt:

```text
net stop w32time
```

Restart it afterwards with `net start w32time`. While it's stopped, nothing keeps this PC's clock in step, which can show up as a steady offset in client results.
