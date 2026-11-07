# Windows Process Creation Investigation — Event ID 4688

## Investigation Scope

The investigation examined Windows Security Event ID 4688 logs in Splunk to review process creation activity.

The investigation covered events recorded during the last 24 hours.

## Splunk Query

```spl
index="main" EventCode="4688" earliest=-24h
| eval creator=lower(Creator_Process_Name), child=lower(New_Process_Name)
| where NOT like(creator,"%\\splunk%")
    AND NOT like(creator,"%\\postgres.exe")
    AND NOT like(child,"%\\splunk%")
    AND NOT like(child,"%\\postgres.exe")
| search Process_Command_Line="C:\\Windows\\system32\\svchost.exe -k wsappx -p -s AppXSvc"
```

## Evidence

### Process Information

- **New Process ID:** `0x2c08`
- **New Process Name:** `C:\Windows\System32\svchost.exe`
- **Creator Process ID:** `0x2b0`
- **Creator Process Name:** `C:\Windows\System32\services.exe`
- **Process Command Line:** `C:\Windows\system32\svchost.exe -k wsappx -p -s AppXSvc`

### File Signature Verification

The authenticity of the executable was verified using PowerShell:

```powershell
Get-AuthenticodeSignature "C:\Windows\System32\svchost.exe"
```

**Output:**

```text
SignerCertificate                         Status    Path
----                                       ------    ----
3B77DB29AC72AA6B5880ECB2ED5EC1EC6601D847  Valid     svchost.exe
```

The signature status was reported as `Valid`.

## Analysis

The logs show an expected parent-child process relationship, where `services.exe` launched `svchost.exe`.

The process command line indicates that `svchost.exe` was started with the AppXSvc service group.

The file signature verification returned a `Valid` status, indicating that the executable is digitally signed and trusted by the operating system.

The available evidence does not establish any signs of process tampering, execution from an unusual location, or suspicious process lineage.

## Verdict

**The observed activity is consistent with normal Windows service activity.**

The process creation event reflects an expected execution of `svchost.exe` by `services.exe`.

The available evidence does not establish that the activity was malicious.