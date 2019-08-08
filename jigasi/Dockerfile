FROM jitsi/base-java

COPY jigasi/version /tmp/

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jigasi=`cat /tmp/version` && \
	apt-cleanup

COPY rootfs/ /

VOLUME /config
