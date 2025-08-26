# DevOps & SSH Fundamentals Cheat Sheet

----------------------------------------------------------------------------------------------------------

## 1. SSH: Secure Shell Basics

SSH (Secure Shell) is a cryptographic protocol for secure remote login and command execution. It encrypts all traffic between the client and server, preventing eavesdropping and man-in-the-middle attacks.  

SSH uses public‐key cryptography: the server presents its host key fingerprint on first connect, and the client stores it in `~/.ssh/known_hosts`. Subsequent connections verify that fingerprint to ensure you’re talking to the right server.

---

## 2. Jump Server (Bastion Host)

A Jump Server is an intermediary host inside the trust boundary. Instead of connecting directly to production servers, you SSH into the Jump Server first. From there, you SSH into target servers. This adds an extra layer of security and auditing.

Typical flow:  
```bash
# From your local machine
ssh thor@jump_host.stratos.xfusioncorp.com

# Then from jump_host
ssh tony@stapp01.stratos.xfusioncorp.com
```

---

## 3. Infrastructure Details

| Server    | IP              | Hostname                              | User   | Password   | Role                        |
|-----------|-----------------|---------------------------------------|--------|------------|-----------------------------|
| stapp01   | 172.16.238.10   | stapp01.stratos.xfusioncorp.com       | tony   | Ir0nM@n    | Nautilus App Server 1       |
| stapp02   | 172.16.238.11   | stapp02.stratos.xfusioncorp.com       | steve  | Am3ric@    | Nautilus App Server 2       |
| stapp03   | 172.16.238.12   | stapp03.stratos.xfusioncorp.com       | banner | BigGr33n   | Nautilus App Server 3       |
| stlb01    | 172.16.238.14   | stlb01.stratos.xfusioncorp.com        | loki   | Mischi3f   | Nautilus HTTP Load Balancer |
| stdb01    | 172.16.239.10   | stdb01.stratos.xfusioncorp.com        | peter  | Sp!dy      | Nautilus Database Server    |
| ststor01  | 172.16.238.15   | ststor01.stratos.xfusioncorp.com      | natasha| Bl@kW      | Nautilus Storage Server     |
| stbkp01   | 172.16.238.16   | stbkp01.stratos.xfusioncorp.com       | clint  | H@wk3y3    | Nautilus Backup Server      |
| stmail01  | 172.16.238.17   | stmail01.stratos.xfusioncorp.com      | groot  | Gr00T123   | Nautilus Mail Server        |
| jump_host | Dynamic         | jump_host.stratos.xfusioncorp.com     | thor   | mjolnir123 | SSH Gateway (Jump Server)   |
| jenkins   | 172.16.238.19   | jenkins.stratos.xfusioncorp.com       | jenkins| j@rv!s     | CI/CD Server                |

---

## 4. SSH Connection Workflow

1. On first connect, accept the host key:
   ```bash
   Are you sure you want to continue connecting (yes/no)? yes
   ```

2. Enter the user’s password exactly (case-sensitive, special characters count).

3. If you see `Permission denied`, verify:
   - Username spelling  
   - Password correctness  
   - Account shell restrictions (e.g., `/sbin/nologin` denies login)

---

## 5. Shell Prompt & Identity

Your shell prompt shows the **current user** and **hostname**:
```bash
tony@stapp01:~$
```

- `tony` is the logged-in user.  
- `stapp01` is the server’s hostname.

If you switch user with `sudo -i`, the prompt becomes:
```bash
root@stapp01:~# 
```

---

## 6. Creating a Non-Interactive User (`ravi`)

On **App Server 1** (`stapp01`), create a backup-agent user with no login shell:

```bash
# From jump_host
ssh tony@stapp01.stratos.xfusioncorp.com
# Enter Ir0nM@n

# On stapp01
sudo useradd -s /sbin/nologin ravi
```

- `-s /sbin/nologin` ensures the user cannot open an interactive shell.

---

## 7. Verifying User Creation

Check `/etc/passwd` for the new entry:

```bash
grep ravi /etc/passwd
```

Expected output:

```
ravi:x:1001:1001::/home/ravi:/sbin/nologin
```

---

## 8. Troubleshooting Tips

- If SSH still fails, try verbose mode:
  ```bash
  ssh -vvv tony@stapp01.stratos.xfusioncorp.com
  ```
- For `sudo` errors, ensure your user is in the `sudoers` file and enter the correct password (`mjolnir123` for `thor`).

---

## Bonus: Next-Level SSH Concepts

- **Key-based Authentication**: Generate `~/.ssh/id_ed25519` and copy your public key to the server’s `~/.ssh/authorized_keys`.
- **SSH Config File**: Simplify host entries in `~/.ssh/config`.
- **Port Forwarding**: Tunnel remote service ports for secure access.
- **Agent Forwarding**: Use your local SSH key on intermediary hosts without copying it.

Dive into these topics to elevate your SSH mastery!
