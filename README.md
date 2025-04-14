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
