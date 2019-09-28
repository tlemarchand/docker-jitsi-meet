FROM debian:stretch-slim

ARG JITSI_RELEASE=stable

COPY base/dl_url_s6-overlay /tmp/
COPY base/dl_url_frep /tmp/

ADD https://download.jitsi.org/jitsi-key.gpg.key /tmp/jitsi.key

COPY base/rootfs /

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y apt-transport-https apt-utils ca-certificates gnupg wget && \
	wget -O /tmp/s6-overlay.tar.gz `cat /tmp/dl_url_s6-overlay` && \
	wget -O /usr/bin/frep `cat /tmp/dl_url_frep` && \
	tar xfz /tmp/s6-overlay.tar.gz -C / && \
	rm -f /tmp/*.tar.gz && \
	apt-key add /tmp/jitsi.key && \
	rm -f /tmp/jitsi.key && \
	echo "deb https://download.jitsi.org $JITSI_RELEASE/" > /etc/apt/sources.list.d/jitsi.list && \
	echo "deb http://ftp.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/backports.list && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get dist-upgrade -y && \
	apt-dpkg-wrap apt-get autoremove -y --purge gnupg wget && \
	apt-cleanup && \
	chmod +x /usr/bin/frep

RUN \
	[ "$JITSI_RELEASE" = "unstable" ] && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jq procps curl vim iputils-ping net-tools && \
	apt-cleanup || \
	true

ENTRYPOINT [ "/init" ]
