# Module for installing certbot for manual DNS challenge verification

See https://www.digitalocean.com/community/tutorials/how-to-acquire-a-let-s-encrypt-certificate-using-dns-validation-with-acme-dns-certbot-on-ubuntu-18-04
for more details.

## Usage

```
certbot certonly --manual --manual-auth-hook /etc/letsencrypt/acme-dns-auth.py --preferred-challenges dns
--debug-challenges -d  the-server-name
```
After prompt, manually add the CNAME record to the DNS and press enter to continue verification.

Renewals are issued automatically via cron task.
