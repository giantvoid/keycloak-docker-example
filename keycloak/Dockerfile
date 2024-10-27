ARG DB_VENDOR
ARG DB_HOST
ARG DB_PORT
ARG DB_DATABASE
ARG DB_USER
ARG DB_PASSWORD
ARG KEYCLOAK_USER
ARG KEYCLOAK_PASSWORD
ARG LOG_LEVEL

#############################################
# Build the Keycloak image
#############################################

FROM quay.io/keycloak/keycloak:latest AS builder

ARG DB_VENDOR

ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true

ENV KC_DB=$DB_VENDOR

WORKDIR /opt/keycloak

# for demonstration purposes only, please make sure to use proper certificates in production instead
RUN keytool -genkeypair -storepass password -storetype PKCS12 -keyalg RSA -keysize 2048 -dname "CN=server" -alias server -ext "SAN:c=DNS:localhost,IP:127.0.0.1" -keystore conf/server.keystore
RUN /opt/keycloak/bin/kc.sh build

#############################################
# Final Keycloak image
#############################################

FROM quay.io/keycloak/keycloak:latest

ARG DB_VENDOR
ARG DB_HOST
ARG DB_PORT
ARG DB_DATABASE
ARG DB_USER
ARG DB_PASSWORD
ARG KEYCLOAK_USER
ARG KEYCLOAK_PASSWORD
ARG LOG_LEVEL

COPY --from=builder /opt/keycloak/ /opt/keycloak/

ENV KC_DB=$DB_VENDOR
ENV KC_DB_URL_HOST=$DB_HOST
ENV KC_DB_URL_PORT=$DB_PORT
ENV KC_DB_URL_DATABASE=$DB_DATABASE
ENV KC_DB_USERNAME=$DB_USER
ENV KC_DB_PASSWORD=$DB_PASSWORD
ENV KC_BOOTSTRAP_ADMIN_USERNAME=$KEYCLOAK_USER
ENV KC_BOOTSTRAP_ADMIN_PASSWORD=$KEYCLOAK_PASSWORD
ENV KC_LOG_LEVEL=$LOG_LEVEL

ENTRYPOINT ["/opt/keycloak/bin/kc.sh"]
