ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base-java

COPY jigasi/version /tmp/

ENV GOOGLE_APPLICATION_CREDENTIALS /config/key.json

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jigasi=`cat /tmp/version` && \
	apt-cleanup

COPY jigasi/rootfs/ /

VOLUME ["/config", "/tmp/transcripts"]
