# Odoo + Postgresql + Traefik

This repository helps set up Odoo 12 + Postgresql 10 served by Traefik 2.0 over HTTPS (Let's Encrypt)

## Setup

Create the `pgdata` folder:

```
mkdir pgdata
```

Then fill the `.env` file with your credentials and your domain:

```
ODOO_USER=
ODOO_PASS=
ODOO_TRAEFIK_URL=
TRAEFIK_DEFAULT_DOMAIN=
ACME_EMAIL=
```

You'll need to set the same credentials inside the `odoo/config/odoo.conf` file:

```
db_user =
db_password =
```


`ODOO_USER` will be used by Odoo to connect to the database (which will be created as well with the `ODOO_PASS` password).
`ODOO_TRAEFIK_URL` is the hostname which Traefik will listen to to route requests to the Odoo container.
`TRAEFIK_DEFAULT_DOMAIN` is the domain part that will be used to compose the defaultRule for Traefik (if you ever need to add new containers to that docker-compose configuration) and will use the following expression (_Name_ being the name of the container):

```
Host(`{{ trimPrefix `/` .Name }}.${TRAEFIK_DEFAULT_DOMAIN}`)
```

`ACME_EMAIL` is the email address which will be used when registering the certificate at Let's Encrypt.

Traefik will automatically renew the certificate every 3 months.

## Run

```bash
cd docker-compose-traefik-odoo-postgres
docker-compose up -d
```

### Tested with
![Docker version](https://img.shields.io/badge/Docker-19.03.2-blue)
![Docker-compose version](https://img.shields.io/badge/Docker--compose-1.18.0-informational)

