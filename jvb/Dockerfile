ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base-java

COPY jvb/version /tmp/

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jitsi-videobridge=`cat /tmp/version` && \
	apt-cleanup

COPY jvb/rootfs/ /

VOLUME /config
