FROM debian:sid-slim

LABEL author phra

ARG BRIDGE_VERSION

ENV BRIDGE_VERSION ${BRIDGE_VERSION}
ENV HORIZON ${HORIZON}
ENV PASSPHRASE ${PASSPHRASE}
ENV DB_TYPE ${DB_TYPE}
ENV DB_URI ${DB_URI}
ENV BRIDGE_PORT ${BRIDGE_PORT}
ENV COMPLIANCE_URL ${COMPLIANCE_URL}
ENV RECEIVE_URL ${RECEIVE_URL}
ENV ISSUER ${ISSUER}
ENV BASE_SEED ${BASE_SEED}
ENV RECEIVING_ACCOUNT ${RECEIVING_ACCOUNT}

WORKDIR /

RUN apt-get update && apt-get dist-upgrade -y && apt-get install wget netcat-openbsd -y

RUN wget -O bridge.tar.gz https://github.com/stellar/bridge-server/releases/download/v${BRIDGE_VERSION}/bridge-v${BRIDGE_VERSION}-linux-amd64.tar.gz

RUN tar xf bridge.tar.gz

WORKDIR /bridge-v${BRIDGE_VERSION}-linux-amd64

RUN chmod u+x bridge

COPY bridge.cfg.sh .

CMD until nc -z postgres_bridge 5432; do sleep 1; done && sh bridge.cfg.sh > bridge.cfg && ./bridge --migrate-db && ./bridge
