# ansible-smtp

Ansible playbook to setup a **SMTP** server, with **dovecot** & **postfix**.


## Run the playbook

```bash
ansible-playbook -i inventory --user=root --become setup-mailserver.yml
```

## Test sending email
```bash
postfix check


sudo apt-get install mailutils -y   # For Debian/Ubuntu
echo "This is a test email" | mail -s "Test Email" -r test@qa-smtp.cyverse.at mojib.wali@tugraz.at




# helm sending emails
helm install exim4  --set secrets.EXIM_SMARTHOST='qa-smtp.cyverse.at:587',secrets.EXIM_PASSWORD='user1@qa-smtp.cyverse.at:{PLAIN}password1',secrets.EXIM_ALLOWED_SENDERS='*'  exim4/exim4 --namespace mail --create-namespace --wait


kubectl exec -it pod/exim4-78788d976b-9vpv7 -n mail -- bash


echo "This is test" | mail -s "The subject" mojib.wali@tugraz.at -aFrom:sender@qa-smtp.cyverse.at



swaks --to mojib.wali@tugraz.at --from user1@qa-smtp.cyverse.at --server qa-smtp.cyverse.at --auth-user user1@qa-smtp.cyverse.at --auth-password password1 --tls

```
