# Portal access troubleshooting guide

## Intake

Record the exact URL, error, start time, last successful use, work blocked, affected users or devices, and available approved workarounds. Clarify whether only this website fails.

## Check and compare

1. Reproduce the reported symptom.
2. Where appropriate for the application, compare HTTP access by known IP and hostname. Production HTTPS certificates, virtual hosts and authentication can make direct-IP tests behave differently, so interpret results in context.
3. Inspect the destination returned by the system resolver. Check relevant local hosts entries and centrally managed DNS records as appropriate.
4. Check evidence before changing anything. A generic browser error does not prove a DNS fault, firewall fault or stopped application.

## Make an authorised correction

Back up configuration and establish ownership and the intended destination. Modify only the relevant entry within your authority. If the setting is centrally managed, unexpected, suspicious or affects several users, escalate with evidence.

## Validate and close

Retest after the approved correction. Confirm the expected page content and original task, not just a successful ping or HTTP header. Document actions, results and user confirmation. Label simulated confirmations explicitly in a lab.

## Commands used in this lab

```powershell
curl.exe --noproxy "*" --connect-timeout 5 -I http://127.0.0.1:8000
curl.exe --noproxy "*" --connect-timeout 5 -I http://staffportal.test:8000
[System.Net.Dns]::GetHostAddresses("staffportal.test") | Select-Object IPAddressToString
Select-String -Path C:\Windows\System32\drivers\etc\hosts -Pattern "staffportal.test"
ipconfig /flushdns
```

The .NET lookup reflects system name resolution; it is not a direct query proving what an external DNS server returned. The fault in this project was in the local hosts file.
