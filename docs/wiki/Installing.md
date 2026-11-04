# Installing

The tool runs on 64-bit Windows 10 and 11. Each release has two downloads holding the same programs:

| Download | What it does |
| --- | --- |
| `NTP-NTS-Diagnostic-Tool-3.1.0-setup.exe` | Installs for you only, adds a Start menu entry, and can be removed in Settings > Apps |
| `NTP-NTS-Diagnostic-Tool-3.1.0-portable.zip` | Nothing to install: unzip it anywhere, including a USB stick, and run `NTP-NTS-Diagnostic-Tool.exe` |

## The installer

Run `NTP-NTS-Diagnostic-Tool-3.1.0-setup.exe`. Its window says what it will install and where, and **Install** starts it. A progress bar shows the files being copied, then the window says the tool is installed, with a box to start it straight away. **Finish** closes it.

It needs no administrator rights, because it installs into your own profile:

    %LOCALAPPDATA%\Programs\NTP-NTS Diagnostic Tool

It puts `NTP-NTS-Diagnostic-Tool.exe`, `ntpnts.exe`, `LICENSE.txt` and `uninstall.exe` there, and adds **NTP-NTS Diagnostic Tool** to the Start menu.

The tool itself asks for Administrator rights only when you want it to change Windows Firewall. See [Windows Firewall and Administrator](Windows-Firewall-and-Administrator).

### Upgrading

Running a newer setup over an older one upgrades it in place: the window names the version you have and the one you're getting, and the button reads **Upgrade**. Your settings are kept. Close the tool first. If it's still running, setup says so and changes nothing, and **Try again** carries on once it's closed.

### Silent install

For scripted installs:

    NTP-NTS-Diagnostic-Tool-3.1.0-setup.exe /S

installs silently. The exit code is 0 on success.

### Using ntpnts from any prompt

The installer doesn't change your `PATH`. To run `ntpnts` from any prompt, add the install folder to your user `PATH`, or call it by its full path.

### SmartScreen

The installer isn't code-signed yet, so Windows SmartScreen may say it "protected your PC". Choose **More info**, then **Run anyway**. To check the file first, see below.

## Checking a download

Each release has a `SHA256SUMS.txt`. In PowerShell:

    Get-FileHash .\NTP-NTS-Diagnostic-Tool-3.1.0-setup.exe

The hash it prints should match the line for that file in `SHA256SUMS.txt`. The file uses lower case, PowerShell upper case.

## Removing it

Use **Settings > Apps > Installed apps > NTP/NTS Diagnostic Tool > Uninstall**, or run `uninstall.exe /U` from the install folder. `uninstall.exe /U /S` removes it silently.

Removing the tool leaves two things alone:

- Your saved settings, in `%APPDATA%\NTP\NTS Diagnostic Tool`. Delete that folder to remove them.
- The Windows Firewall rule **NTP-NTS Diagnostic Tool Peer Responder**, if you added one. Remove it with **Remove rule** in the tool before uninstalling, or later in Windows Defender Firewall.
