FROM nginx:1.17

ARG JITSI_RELEASE=stable

ENV JICOFO_AUTH_USER=focus
ENV XMPP_DOMAIN=meet.jitsi
ENV XMPP_AUTH_DOMAIN=auth.meet.jitsi
ENV XMPP_BOSH_URL_BASE=http://prosody:5280
ENV XMPP_MUC_DOMAIN=muc.meet.jitsi

ADD https://download.jitsi.org/jitsi-key.gpg.key /tmp/jitsi.key
COPY web-ng/version /tmp/
COPY web-ng/gzip.conf /etc/nginx/conf.d/
COPY web-ng/jitsi-meet.conf.template /usr/share/templates/
COPY web-ng/config.js.template /usr/share/templates/

RUN apt-key add /tmp/jitsi.key && \
    rm -f /tmp/jitsi.key && \
    echo "deb https://download.jitsi.org $JITSI_RELEASE/" > /etc/apt/sources.list.d/jitsi.list && \
    apt-get update && apt-get install -y jitsi-meet-web=`cat /tmp/version` && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -f /etc/nginx/conf.d/default.conf && \
    mkdir -p /etc/jitsi/meet/

CMD /bin/bash -c "envsubst < /usr/share/templates/jitsi-meet.conf.template > /etc/nginx/conf.d/jitsi-meet.conf && \
                  envsubst < /usr/share/templates/config.js.template > /etc/jitsi/meet/config.js && \
                  exec nginx -g 'daemon off;'"
