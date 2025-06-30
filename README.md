# ansible-smtp

Ansible playbook to setup a **SMTP** server, with:

[DOVECOT](https://www.dovecot.org/) is an open-source IMAP and POP3 server for email.

[Postfix](https://www.postfix.org/) is a free and open-source mail transfer agent (MTA) that routes and delivers electronic mail.

## Run the playbook

```bash
ansible-playbook -i inventory --user=root --become setup-mailserver.yml
```


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
