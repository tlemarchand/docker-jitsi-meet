FROM nginx:1.17

ARG JITSI_RELEASE=stable

ENV JICOFO_AUTH_USER=focus \
    XMPP_DOMAIN=meet.jitsi \
    XMPP_AUTH_DOMAIN=auth.meet.jitsi \
    XMPP_BOSH_URL_BASE=http://prosody:5280 \
    XMPP_MUC_DOMAIN=muc.meet.jitsi

ADD https://download.jitsi.org/jitsi-key.gpg.key /tmp/jitsi.key
COPY web-ng/version /tmp/
COPY web-ng/gzip.conf /etc/nginx/conf.d/
COPY web-ng/jitsi-meet.conf.template /usr/share/templates/

RUN apt-get update && \
    apt-get install -y apt-transport-https apt-utils ca-certificates gnupg && \
    apt-key add /tmp/jitsi.key && \
    rm -f /tmp/jitsi.key && \
    mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled && \
    echo "deb https://download.jitsi.org $JITSI_RELEASE/" > /etc/apt/sources.list.d/jitsi.list && \
    apt-get update && apt-get install -y jitsi-meet-web=`cat /tmp/version` jitsi-meet-web-config && \
    apt-get autoremove --purge -y gnupg && \
    rm -rf /var/lib/apt/lists/* && \
    rm -f /etc/nginx/conf.d/default.conf && \
    mkdir -p /srv/jitsi/meet/config && \
    mv /usr/share/jitsi-meet/interface_config.js /srv/jitsi/meet/config/ && \
    cp /usr/share/jitsi-meet-web-config/config.js /usr/share/templates/config.js.template && \
    sed -i \
        -e "s#jitsi-meet.example.com#\$\{XMPP_DOMAIN\}#g" \
        -e "s#bosh:.*#bosh: '/http-bind',#" \
        -e "s#muc:.*#muc: '\$\{XMPP_MUC_DOMAIN\}',#" \
        -e "s#// focusUserJid:.*#focusUserJid: '\$\{JICOFO_AUTH_USER\}@\$\{XMPP_AUTH_DOMAIN\}',#" \
        -e "s#enabled: true,#enabled: false,#" \
        -e "s#// constraints: {#constraints: {#" \
        -e "s#//     video: {#    video: {#" \
        -e "s#//         height: {#        height: {#" \
        -e "s#//             ideal: 720,#            ideal: 360,#" \
        -e "s#//             max: 720,#            max: 360,#" \
        -e "s#//             min: 240#            min: 240\n            }\n        }\n    },#" \
        /usr/share/templates/config.js.template

CMD envsubst '${XMPP_BOSH_URL_BASE} ${XMPP_DOMAIN}' < /usr/share/templates/jitsi-meet.conf.template > /etc/nginx/conf.d/jitsi-meet.conf && envsubst < /usr/share/templates/config.js.template > /srv/jitsi/meet/config/config.js && exec nginx -g 'daemon off;'
