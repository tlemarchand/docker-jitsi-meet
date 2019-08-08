FROM jitsi/base-java

COPY jvb/version /tmp/

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jitsi-videobridge=`cat /tmp/version` && \
	apt-cleanup

COPY rootfs/ /

VOLUME /config

