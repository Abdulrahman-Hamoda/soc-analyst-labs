
# SOC Investigation Report — Basic Process Execution

## 1. Investigation Scope

- The investigation examined Windows Security logs for process execution events.
- The investigation covered events recorded on November 2 and November 3, 2026.
- The investigation focused on Event ID 4688, process relationships, and command-line activity.

---

## 2. Scenario

- The Windows host generated several process execution events.
- The objective was to determine whether the process execution activity was expected or suspicious.
- The activity was identified during a review of process creation events.

---

## 3. Investigation Methodology

- The investigation was conducted using Splunk searches on Event ID 4688 to identify process execution activity.
- The analysis included reviewing the Parent Process, New Process, and Command Line fields.

---

## 4. Evidence

- The logs recorded process creation events involving `cmd.exe`, `conhost.exe`, `whoami.exe`, and `net.exe`.
- The relevant fields showed `svc_update.exe` as the creator of `cmd.exe`, followed by command execution through `cmd.exe`.

### November 2, 2026

| Time | Parent Process | Child Process |
|---|---|---|
| 14:14:17.565 | `explorer.exe` | `svc_update.exe` |
| 14:20:22.062 | `svc_update.exe` | `cmd.exe` |
| 14:20:22.083 | `cmd.exe` | `conhost.exe` |
| 14:21:07.812 | `cmd.exe` | `whoami.exe` |

### November 3, 2026

- Similar process execution activity was recorded between `14:44:02.157` and `14:47:12.880`.

Additional events:

| Time | Parent Process | Child Process | Command Line |
|---|---|---|---|
| 14:49:05.189 | `cmd.exe` | `net.exe` | `net user` |
| 14:49:05.472 | `net.exe` | `net1.exe` | `net1 user` |
| 15:09:49.708 | `cmd.exe` | `net.exe` | `net localgroup` |

---

## 5. Analysis

- The logs show `explorer.exe` created `svc_update.exe`, which subsequently created `cmd.exe`.
- The `cmd.exe` process then initiated `net.exe`, which was used to execute the `net user` and `net localgroup` commands.
- This pattern is consistent with process execution followed by account and local-group discovery through `svc_update.exe`.
- The Parent Process field identifies `svc_update.exe` as the parent of `cmd.exe` and helps establish the subsequent process chain.
- The available evidence does not establish whether the file performed additional actions beyond the observed discovery commands.

---

## 6. Verdict

- The observed activity is consistent with suspicious post-execution discovery behavior.
- No evidence of privilege escalation was identified within the reviewed data.

---

## 7. Recommended Actions

- Review Event ID `4624` to determine whether a successful logon occurred afterward.
- Review Event ID `4672` to determine whether special privileges were assigned to a new logon.
- Review Event ID `4732` to determine whether a member was added to a local security group.
- Review Event ID `4720` to determine whether a user account was created.
- Identify and verify the host associated with the source IP address to determine whether the activity was authorized.
- Correlate process creation events with network telemetry to determine whether the process initiated a network connection.

---

## Lab Context Note

The simulation was continued in a later lab session after the environment had been stopped. The simulation file was executed again and a new access session was established, so the CMD creation event appears again. This shutdown is not treated as activity performed by the attacker.
