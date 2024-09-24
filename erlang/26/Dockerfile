FROM        --platform=$TARGETOS/$TARGETARCH erlang:26-alpine

LABEL       author="Embotic" maintainer="luke@embotic.xyz"

LABEL       org.opencontainers.image.source="https://github.com/hypernet-org.uk/embotic-xyz"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apk add --update --no-cache ca-certificates curl git openssl sqlite tar tzdata iproute2 \
				&& adduser -D -h /home/container container \
				&& rm -rf /usr/bin/dd \
				&& rm -rf /usr/bin/fallocate \
				&& rm -rf /usr/bin/truncate \
				&& rm -rf /usr/bin/xfs_mkfile

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
