# docker-harmony
Bugzilla Harmony/BMO compose script with Traefik as reverse proxy + MariaDB instead of MySQL.

The example handles the following domains:
- `http[s]://bugzilla.localhost` automatic https redirection

Note: You may require a local DNS / patch of `/etc/hosts` to get the subdomains working.
## Initial checkout
```bash
git clone <url>
# e.g. git clone git@github.com:userid0x0/docker-harmony.git
# e.g. git clone https://github.com/userid0x0/docker-harmony.git
cd docker-harmony
git submodule init
git submodule update
```
## Create certificate
This example uses [mkcert](https://github.com/FiloSottile/mkcert) to generate wildcard certificate for `*.localhost`.
For private use you may use this approach to create a certificate for your local router environment e.g. `*.host.localdomain`
```bash
wget -O mkcert https://dl.filippo.io/mkcert/latest?for=linux/amd64
chmod +x mkcert
mkdir certs
./mkcert -cert-file certs/_wildcard.localhost.pem -key-file certs/_wildcard.localhost-key.pem "*.localhost" localhost 127.0.0.1 ::1
```
the output might look as follows
```
Created a new local CA
Note: the local CA is not installed in the system trust store.
Note: the local CA is not installed in the Firefox and/or Chrome/Chromium trust store.
Run "mkcert -install" for certificates to be trusted automatically

Created a new certificate valid for the following names
 - "*.localhost"
   Warning: many browsers don't support second-level wildcards like "*.localhost"
 - "localhost"
 - "127.0.0.1"
 - "::1"

Reminder: X.509 wildcards only go one level deep, so this won't match a.b.localhost

The certificate is at "certs/_wildcard.localhost.pem" and the key at "certs/_wildcard.localhost-key.pem"

It will expire on ...
```
You may run `./mkcert -CAROOT` to see where the RootCA is stored e.g. to move it to other devices. For more information check the mkcert project page.

The generated files are used by `providers/tls.yml`.

## Startup
```bash
docker network create traefik_default
docker compose up
```
