# Keycloak Examples

A big repo with all the interesting keycloak examples/starters i've needed!

## keycloak useful config

### Adding postgres for persistant storage
Other providers are available and configured in the same way... I use postgres because I use it for everything else and will most likely be my target DB

Add the following env vars to the dockerfile
``` docker
ENV DB_VENDOR=POSTGRES
ENV DB_ADDR=postgres
ENV DB_SCHEMA=public
ENV DB_DATABASE=keycloak
ENV DB_USER=keycloak
ENV DB_PASSWORD=password
```

Add a volume and service to the docker-compose.yml
``` yaml
volumes:
  postgres_data:
    driver: local
services:
  postgres:
    image: postgres
    container_name: kc_examples_postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: password
    ports:
      - 15432:5432
```

### Loading realms and clients on start up
Loading multiple realms and clients on startup (even over writing the master), great for bringing up a pre configured version of keycloak, for sharing with dev teams etc but maybe not for a production system, especially one backed with persistant storage.

Yeah this could be done by copying the files with the COPY command in the dockerfile, but as this file might be used for a production build and I wouldn't load the config on start up, I've opted for a compose volume mount...

Map a volume to the realm files in the docker-compose.yml
``` yaml
volumes:
    - ./keycloak/config/realms/:/realms
```
Overload the the docker CMD in the dockerfile with
``` docker
CMD ["-b", "0.0.0.0", "-Dkeycloak.migration.action=import", "-Dkeycloak.migration.provider=dir", "-Dkeycloak.migration.dir=/realms/", "-Dkeycloak.migration.strategy=OVERWRITE_EXISTING"]
```