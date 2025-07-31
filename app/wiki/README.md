# Wiki.js

Wiki.js : https://js.wiki/

The most powerful and extensible open source Wiki software.

You can serve wiki with external PostgreSQL database.


## TL;DR

Check configuration in [`.env`](.env), then launch wiki.js with

```bash
cd app/wiki ; make up
```

Check http://10.10.10.10 or http://wiki.pigsty and following the wizard.

Public demo: http://wiki.pigsty.cc , username: `admin@pigsty.cc` , password: `pigsty.wiki`


## Database

```yaml
# postgres://dbuser_wiki:DBUser.Wiki@10.10.10.10:5432/wiki
- { name: wiki, owner: dbuser_wiki, revokeconn: true , comment: wiki the api gateway database }
- { name: dbuser_wiki, password: DBUser.Wiki , pgbouncer: true , roles: [ dbrole_admin ] }
```

```bash
bin/pgsql-user pg-meta dbuser_wiki
bin/pgsql-db   pg-meta wiki
```

## Docker

```yaml

services:
  wiki:
    container_name: wiki
    image: requarks/wiki:2
    environment:
      DB_TYPE: postgres
      DB_HOST: 10.10.10.10
      DB_PORT: 5432
      DB_USER: dbuser_wiki
      DB_PASS: DBUser.Wiki
      DB_NAME: wiki
    restart: unless-stopped
    ports:
      - "9002:3000"
```

## Access

* Default Port for wiki: 9002

```yaml
# add to nginx_upstream
- { name: wiki  , domain: wiki.pigsty.cc , endpoint: "127.0.0.1:9002"   }
```

```bash
./infra.yml -t nginx_config
ansible all -b -a 'nginx -s reload'
```
