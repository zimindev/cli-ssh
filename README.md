# **📚 Complete SSH Guide for Linux 🐧🔐**

Connect to remote servers and network devices securely using **SSH (Secure Shell)** from the Linux command line.

This guide covers **connecting, custom ports, keys, troubleshooting, file transfer, tunneling, and useful SSH options**.

---

## **🛠️ Installation**

### **Ubuntu/Debian 🐳**

```bash
sudo apt update
sudo apt install openssh-client
```

### **Fedora/RHEL 🎩**

```bash
sudo dnf install openssh-clients
```

### **Arch Linux/Manjaro 🏗️**

```bash
sudo pacman -S openssh
```

### **OpenSUSE 🦎**

```bash
sudo zypper install openssh-clients
```

---

## **🏁 Basic SSH Connection**

Connect to a remote device:

```bash
ssh username@192.168.1.1
```

Example with a MikroTik router:

```bash
ssh admin@192.168.88.1
```

General syntax:

```bash
ssh [options] username@hostname
```

---

## **🔌 Connect Using a Custom Port**

The default SSH port is:

```text
22
```

Connect using another port:

```bash
ssh -p 2222 username@192.168.1.1
```

Example:

```bash
ssh -p 2200 admin@192.168.88.1
```

---

## **🌐 Connect Using a Hostname**

You can connect using a hostname instead of an IP:

```bash
ssh user@server.local
```

Or a domain:

```bash
ssh user@example.com
```

---

## **🧪 Test SSH Connectivity**

Check if the device is reachable:

```bash
ping 192.168.1.1
```

Check whether SSH port 22 is open:

```bash
nc -zv 192.168.1.1 22
```

Or:

```bash
nmap -p 22 192.168.1.1
```

---

## **🔐 SSH Keys**

### **Generate an SSH Key**

Recommended modern key type:

```bash
ssh-keygen -t ed25519
```

Press `Enter` to use the default location:

```text
~/.ssh/id_ed25519
```

Your public key will be:

```text
~/.ssh/id_ed25519.pub
```

---

## **📤 Copy Public Key to a Server**

```bash
ssh-copy-id username@192.168.1.100
```

After that, connect:

```bash
ssh username@192.168.1.100
```

You should no longer need to enter the account password if key authentication is configured correctly.

---

## **🔑 Use a Specific SSH Key**

```bash
ssh -i ~/.ssh/id_ed25519 username@192.168.1.100
```

Example:

```bash
ssh -i ~/.ssh/mikrotik_ed25519 admin@192.168.88.1
```

---

## **👤 Specify a Different Username**

```bash
ssh root@192.168.1.100
```

or:

```bash
ssh administrator@192.168.1.100
```

---

## **🐛 SSH Debug Mode**

If the connection does not work, enable verbose output:

```bash
ssh -v username@192.168.1.100
```

More detailed:

```bash
ssh -vv username@192.168.1.100
```

Maximum debugging:

```bash
ssh -vvv username@192.168.1.100
```

Useful for diagnosing:

* 🔌 Connection problems
* 🔐 Authentication errors
* 🔑 SSH key problems
* 🌐 Network issues
* 🚪 Incorrect ports

---

## **⚡ Run a Remote Command**

You don't have to open an interactive SSH session.

Run a command remotely:

```bash
ssh username@192.168.1.100 "hostname"
```

Example:

```bash
ssh admin@192.168.88.1 "/system identity print"
```

Run multiple commands:

```bash
ssh username@192.168.1.100 "command1; command2"
```

---

## **🖥️ SSH Config File**

Create or edit:

```bash
nano ~/.ssh/config
```

Example:

```text
Host mikrotik
    HostName 192.168.88.1
    User admin
    Port 22
```

Now you can simply use:

```bash
ssh mikrotik
```

Another example:

```text
Host my-server
    HostName 192.168.1.100
    User domino
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Connect with:

```bash
ssh my-server
```

---

## **📁 SSH File Transfer**

### **Copy File to Remote Host**

```bash
scp file.txt username@192.168.1.100:/home/username/
```

### **Copy File from Remote Host**

```bash
scp username@192.168.1.100:/home/username/file.txt .
```

### **Copy a Directory**

```bash
scp -r myfolder/ username@192.168.1.100:/home/username/
```

---

## **📦 SFTP**

Start an interactive SFTP session:

```bash
sftp username@192.168.1.100
```

Useful commands:

```text
ls
pwd
cd
get file.txt
put file.txt
mkdir folder
rm file.txt
exit
```

---

## **🚇 SSH Port Forwarding**

### **Local Port Forwarding**

Forward local port `8080` to remote port `80`:

```bash
ssh -L 8080:localhost:80 username@192.168.1.100
```

Then open:

```text
http://localhost:8080
```

### **Remote Port Forwarding**

```bash
ssh -R 8080:localhost:80 username@192.168.1.100
```

### **SOCKS Proxy**

Create a SOCKS proxy:

```bash
ssh -D 1080 username@192.168.1.100
```

---

## **🔄 Keep SSH Connection Alive**

Useful for unstable connections:

```bash
ssh -o ServerAliveInterval=60 username@192.168.1.100
```

With timeout:

```bash
ssh -o ServerAliveInterval=60 \
    -o ServerAliveCountMax=3 \
    username@192.168.1.100
```

---

## **🚪 Exit SSH**

Inside an SSH session:

```bash
exit
```

or:

```text
Ctrl+D
```

---

## **🆘 SSH Escape Commands**

SSH has special commands available through:

```text
Enter
~
```

For example, terminate a frozen SSH connection:

```text
Enter
~
.
```

The sequence is:

```text
Enter → ~ → .
```

Other useful escape commands:

```text
~?
```

Show available SSH escape commands.

```text
~#
```

Show forwarded connections.

```text
~^Z
```

Suspend the SSH session.

---

## **🧹 Clear the Terminal**

Clear the local terminal:

```bash
clear
```

Or:

```text
Ctrl+L
```

---

## **🔎 Check Known SSH Hosts**

SSH stores known host fingerprints in:

```bash
~/.ssh/known_hosts
```

View them:

```bash
cat ~/.ssh/known_hosts
```

Remove a specific host:

```bash
ssh-keygen -R 192.168.1.100
```

Example:

```bash
ssh-keygen -R 192.168.88.1
```

---

## **⚠️ Common SSH Errors**

### **Connection Refused**

```text
ssh: connect to host 192.168.1.100 port 22: Connection refused
```

Possible causes:

* SSH service is disabled.
* SSH is running on another port.
* Firewall is blocking the connection.
* Wrong IP address.

---

### **Connection Timed Out**

```text
Connection timed out
```

Check:

```bash
ping 192.168.1.100
```

Then:

```bash
nmap -p 22 192.168.1.100
```

---

### **Permission Denied**

```text
Permission denied (publickey,password)
```

Check:

* Username
* Password
* SSH key
* `~/.ssh/authorized_keys`
* SSH server authentication settings

---

### **Host Key Changed**

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

If you know the device was legitimately reinstalled or its SSH keys changed:

```bash
ssh-keygen -R 192.168.1.100
```

Then reconnect:

```bash
ssh username@192.168.1.100
```

> ⚠️ Do not blindly ignore this warning. An unexpected host-key change can indicate a security problem.

---

## **🎮 Quick Command Reference**

| Command                              | Action                    |
| ------------------------------------ | ------------------------- |
| `ssh user@host`                      | Connect via SSH           |
| `ssh -p 2222 user@host`              | Connect using custom port |
| `ssh -i key user@host`               | Use specific SSH key      |
| `ssh -v user@host`                   | Debug SSH connection      |
| `ssh -vvv user@host`                 | Maximum debugging         |
| `ssh user@host "command"`            | Run remote command        |
| `ssh-copy-id user@host`              | Copy public key           |
| `scp file user@host:/path/`          | Upload file               |
| `scp user@host:/path/file .`         | Download file             |
| `sftp user@host`                     | Start SFTP                |
| `ssh -L 8080:localhost:80 user@host` | Local port forwarding     |
| `ssh -D 1080 user@host`              | SOCKS proxy               |
| `ssh-keygen -R host`                 | Remove known host         |
| `exit`                               | Close SSH session         |

---

## **🚀 Recommended SSH Workflow**

```bash
# 1. Check connectivity
ping 192.168.1.100

# 2. Check SSH port
nc -zv 192.168.1.100 22

# 3. Connect
ssh username@192.168.1.100

# 4. If connection fails, enable debugging
ssh -vvv username@192.168.1.100

# 5. Exit
exit
```

---

## 📚 **Additional Resources**

* **OpenSSH Documentation**: https://www.openssh.com/manual.html
* **OpenSSH Manual Pages**: https://man.openbsd.org/ssh
* **OpenSSH Portable**: https://www.openssh.com/
