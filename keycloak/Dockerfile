FROM jboss/keycloak:10.0.1

ENV KEYCLOAK_USER=admin
ENV KEYCLOAK_PASSWORD=password
ENV DB_VENDOR=POSTGRES
ENV DB_ADDR=postgres
ENV DB_SCHEMA=public
ENV DB_DATABASE=keycloak
ENV DB_USER=keycloak
ENV DB_PASSWORD=password

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 CMD curl --fail http://localhost:8080/auth/realms/master

CMD ["-b", "0.0.0.0", "-Dkeycloak.migration.action=import", "-Dkeycloak.migration.provider=dir", "-Dkeycloak.migration.dir=/realms/", "-Dkeycloak.migration.strategy=OVERWRITE_EXISTING", "-Dkeycloak.profile.feature.admin_fine_grained_authz=enabled"]
