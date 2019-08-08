FROM jitsi/base

COPY base/90prosody-backports /etc/apt/preferences.d/
COPY prosody/version /tmp/

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -t stretch-backports -y prosody=`cat /tmp/version` && \
	apt-cleanup && \
	rm -rf /etc/prosody

COPY prosody/rootfs/ /

EXPOSE 5222 5269 5347 5280

VOLUME ["/config", "/prosody-plugins-custom"]
