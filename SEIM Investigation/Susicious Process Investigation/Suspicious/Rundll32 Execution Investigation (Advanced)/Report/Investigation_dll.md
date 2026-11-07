# Process Execution Investigation

## Investigation Scope

- The investigation examined Windows Security Logs related to process execution.
- The investigation covered recorded events on November 3, 2026.
- The investigation focused on process creation events (Event ID 4688) and command-line activity.

## Scenario

- The Windows host generated multiple process execution events.
- The objective was to determine whether the execution was expected and authorized or potentially suspicious.
- The activity was observed during the review of process creation events.

## Investigation Methodology

- The investigation was conducted using Splunk searches on Event ID 4688 to identify process execution.
- The analysis included reviewing parent-child process relationships and command-line fields.

---

## Evidence

### Observed Process Execution Chain

`explorer.exe → cmd.exe → rundll32.exe → cmd.exe → Discovery Commands`

The execution of `rundll32.exe` involved the following DLL:

`C:\Users\abdo\Downloads\svc_update.dll`

### Correlated Process Relationships

| Parent Process | Child Process | Timestamp | Command Line |
|---|---|---|---|
| `explorer.exe` | `cmd.exe` | 21:47:31.995 | Not specified |
| `cmd.exe` | `rundll32.exe` | 21:48:44.109 | `rundll32.exe C:\Users\abdo\Downloads\svc_update.dll,0` |
| `rundll32.exe` | `cmd.exe` | 21:52:24.192 | `cmd.exe` |
| `cmd.exe` | `whoami.exe` | 21:52:56:547 | `whoami /all` |
| `cmd.exe` | `ipconfig.exe` | 21:53:40.905 | `ipconfig /all` |
| `cmd.exe` | `tasklist.exe` | 21:53:55.222 | `tasklist` |
| `cmd.exe` | `net.exe` | 21:54:37.624 | `net user` |
| `cmd.exe` | `net.exe` | 21:55:12.175 | `net localgroup Administrators` |

## Analysis

- The logs show that `cmd.exe` launched `rundll32.exe` with the command line `rundll32.exe C:\Users\abdo\Downloads\svc_update.dll,0`.
- After that, `rundll32.exe` spawned another `cmd.exe` process, which executed discovery commands, including `whoami /all`, `tasklist`, `net user`, `ipconfig /all`, and `net localgroup Administrators`.
- This pattern is consistent with potentially suspicious process execution and Discovery activity.
- The available evidence does not establish whether the file performed additional malicious actions.

## Verdict

- The activity is consistent with command execution initiated through the suspicious file.
- No evidence of privilege escalation was identified within the reviewed data.

## Recommended Actions

- Review the following events to determine whether any related activity occurred afterward:
  - Event ID 4624 to identify successful logons.
  - Event ID 4672 to determine whether special privileges were assigned to a new logon.
  - Event ID 4732 to determine whether a member was added to a local security group.
  - Event ID 4720 to determine whether a user account was created.
- Identify and verify the host associated with the source IP address to determine whether the activity was authorized.
- Correlate process creation events with network telemetry to establish whether the process initiated a connection.