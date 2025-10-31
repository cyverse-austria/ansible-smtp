# 📨 ansible-smtp

Ansible playbook to set up an **SMTP mail server** with the following components:

- **[Postfix](https://www.postfix.org/):** A free and open-source Mail Transfer Agent (MTA) that routes and delivers email.  
- **[Dovecot](https://www.dovecot.org/):** An open-source IMAP and POP3 server for email retrieval and storage.

---

## 🚀 Run the Playbook

Use the following command to set up your mail server:

```bash
ansible-playbook -i inventory --user=root --become setup-mailserver.yml --tag setup
```

### 🧩 Service Status
Check whether the mail services are running:

```bash
systemctl status dovecot
systemctl status postfix
```

## 🧪 Testing (Development Mode)
When developing or testing, you can easily destroy the host environment:

```bash
ansible-playbook -i inventory --user=root --become setup-mailserver.yml --tag destroy
```

> ⚠️ **Note:**  
> Running the playbook **without specifying a tag** will execute both `setup` and `destroy` tasks, effectively canceling each other out.


## 📧 Test Sending Email (Local)
Install mail utilities and send a test email:

```bash
sudo apt-get install mailutils -y   # For Debian/Ubuntu
echo "This is a test email" | mail -s "Test Email" -r test@smtp.domain.at me@example.com
```

## 🌐 Test Sending Email from External Services

1. Set your desired test user in `defaults/main.yml`.
2. On your local client machine, run:

```bash
sudo apt-get install swaks -y  # For Debian/Ubuntu
swaks -t me@example.com -s mail.example.com:587 -tls -a LOGIN
```

You’ll be prompted for a username and password — enter the credentials of your test user.
If successful, an email should arrive at the specified destination address.

## 🧰 Debugging and Logs

Check recent logs for Dovecot or Postfix:

```bash
journalctl -u dovecot --since "10 minutes ago"
journalctl -u postfix --since "10 minutes ago"
```

If you’re using Let’s Encrypt for certificates, view the Certbot logs here:

```bash
cat /var/log/letsencrypt/letsencrypt.log
```

## 🔐 Allowed IPs
You can configure which IPs are allowed to relay mail through the server by editing:

```bash
roles/mailserver/defaults/main.yml
```

The variable that controls this is `mynetworks`.

This is used both directly for the `permit_mynetworks` restriction and to generate the Postfix access table.


### 💡 Examples:

```bash
# Localhost + Private subnets
mynetworks: 127.0.0.0/8 163.221.23.0/20 11.12.0.0/16 13.109.0.0/20

# IPv4 + IPv6 localhost
mynetworks: 127.0.0.0/8 [::1]/128

# Public IP
mynetworks: 122.21.216.176

```

> 🧭 **Tip:**  
> - Use **private IPs** if the client connects directly or through VPN.  
> - Use **public IPs** if the client connects over the internet.  
> - Both **IPv4**, **IPv6**, and **subnet ranges** are supported.
