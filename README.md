# ansible-smtp

Ansible playbook to setup a **SMTP** server, with:

[DOVECOT](https://www.dovecot.org/) is an open-source IMAP and POP3 server for email.

[Postfix](https://www.postfix.org/) is a free and open-source mail transfer agent (MTA) that routes and delivers electronic mail.

## Run the playbook

```bash
ansible-playbook -i inventory --user=root --become setup-mailserver.yml --tag setup
```

## Testing only - with tags

When developing, it's useful to easily clear the host environment, for this just run the same playbook command as above, just use ```--tag destroy```.

### Note: Not providing a tag will lead to the execution of the whole playbook, so setup and then destroy, which will cancel each other out.


## Test sending email
```bash
sudo apt-get install mailutils -y   # For Debian/Ubuntu
echo "This is a test email" | mail -s "Test Email" -r test@qa-smtp.cyverse.at mojib.wali@tugraz.at
```

## Test sending email from external services
1. Set your desired test user in defaults/main.yml
2. Run the following commands in the local client
```bash
sudo apt-get install swaks # For Debian/Ubuntu
swaks -t enitu@tugraz.at -s mail.example.com:587 -tls -a LOGIN
```
This will prompt the login interface with username and password. Enter the credentials of the test created user. After running the _swaks_ command a mail should arrive at the given destination.

## Debug logs
Log in to the mail server host and type these commands to debug failed runs:

```journalctl -u <service> --since "10 minutes ago"``` where service could be: **postfix** or **dovecot**.

```/var/log/letsencrypt/letsencrypt.log``` for certbot fails.

## Allowed IPs

Configure at _/roles/mailserver/defaults_ which IPs are allowed to relay mails through the mail server. If the client host is directly connected to the server, or through VPN, the IP entered should be the **private** IP, else if the client and server are connected through the internet, add the **public** IP. Both IPv4 and IPv6 are accepted, and even subnets.

Variable that holds this config is **mynetworks**. This is used both directly for _permit_mynetworks_ restriction and is also processed in a template to create an access
table.

Examples:

mynetworks: 127.0.0.0/8 163.221.23.0/20 11.12.0.0/16 13.109.0.0/20

mynetworks: 127.0.0.0/8 [::1]/128

mynetworks: 122.21.216.176
