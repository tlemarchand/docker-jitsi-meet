FROM jitsi/base-java

COPY jicofo/version /tmp/

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jicofo=`cat /tmp/version` && \
	apt-cleanup

COPY jicofo/rootfs/ /

VOLUME /config
