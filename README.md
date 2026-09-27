# 🛡️ Writeup: Pickle Rick (TryHackMe)
**Author:** Eliaz Andry 
**Difficulty:** Easy  
**Target:** Linux (Ubuntu)  

## 📋 1. Executive Summary
"Pickle Rick" is a Rick and Morty-themed CTF machine. The objective is to find three ingredients to help Rick make a potion. The machine is vulnerable to credential exposure via web source code and `robots.txt`, leading to a web shell. Privilege escalation is achieved through a misconfigured `sudo` permission allowing the `www-data` user to execute commands as root without a password.

---

## 🔍 2. Reconnaissance
I started by scanning all TCP ports to identify running services using Nmap:

```bash
nmap -sV -sC -p- -T4 10.130.129.91
```

**Key Findings:**
- **Port 22/tcp**: OpenSSH 8.2p1 (Ubuntu)
- **Port 80/tcp**: Apache httpd 2.4.41 (Ubuntu)

---

## 🌐 3. Web Enumeration & Initial Access
I navigated to `http://10.130.129.91/` and inspected the page source code (`CTRL+U`). 
I found a hidden comment containing the first piece of the puzzle:
> *"Note to self, remember username! Username: R1ckRul3s"*

Next, I ran a directory brute-force attack using `dirb` (or `gobuster`), which revealed `/login.php`, `/assets/`, and `/robots.txt`.

Checking `http://10.130.129.91/robots.txt`, I found the password:
> *"Wubbalubbadubdub"*

I navigated to `/login.php`, entered the credentials (`R1ckRul3s` / `Wubbalubbadubdub`), and successfully gained access to a web-based command panel.

---

## 👑 4. Privilege Escalation
Once inside the web panel, I executed basic commands to enumerate the system. 
I listed the current directory and found the first flag:
```bash
ls
# Output includes: Sup3rS3cretPickl3Ingred.txt
```
I read the first ingredient:
```bash
less Sup3rS3cretPickl3Ingred.txt
# 1st ingredient: mr. meeseek hair
```

Knowing I was running as the `www-data` user, I checked for sudo privileges:
```bash
sudo -l
```
*(Note: Even if not explicitly shown in the raw notes, the ability to run `sudo less` implies NOPASSWD or a specific sudoers misconfiguration for the `www-data` user).*

I verified I could read root-owned files:
```bash
sudo ls -la /root
```
The output revealed a file named `3rd.txt`. I read it using `sudo less` to bypass standard user restrictions:
```bash
sudo less /root/3rd.txt
# 3rd ingredient: fleeb juice
```

Finally, I checked the `/home/rick` directory and found the second ingredient:
```bash
less /home/rick/"second ingredients"
# 2nd ingredient: 1 jerry tear
```

---

## 🏁 5. Flags Summary
| Flag | Location | Content |
| :--- | :--- | :--- |
| **1st Ingredient** | `/var/www/html/Sup3rS3cretPickl3Ingred.txt` | `mr. meeseek hair` |
| **2nd Ingredient** | `/home/rick/second ingredients` | `1 jerry tear` |
| **3rd Ingredient** | `/root/3rd.txt` | `fleeb juice` |

---

## 🛡️ 6. Remediation & Recommendations
1. **Never hardcode credentials** in HTML source code or `robots.txt`. These files are publicly accessible.
2. **Restrict Sudo Privileges**: The `www-data` user should never have passwordless `sudo` access, especially not to powerful pagers like `less`, `more`, or `vim`, which can be easily exploited to spawn a root shell.
3. **Hide Sensitive Files**: Configuration files and flags should not be placed in world-readable web directories (`/var/www/html`).
