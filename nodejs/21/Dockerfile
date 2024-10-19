FROM        --platform=$TARGETOS/$TARGETARCH node:21-bookworm-slim

LABEL       author="Embotic" maintainer="luke@embotic.xyz"

RUN         apt update \
            && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
            && useradd -m -d /home/container container \
            && rm -rf /usr/bin/dd \
            && rm -rf /usr/bin/fallocate \
            && rm -rf /usr/bin/truncate \
            && rm -rf /usr/bin/xfs_mkfile

RUN echo '#!/bin/bash \n\
if [ -d "/home/container/node_modules/discord.js-selfbot-v13" ]; then \n\
    echo "Selfbot package folder \'discord.js-selfbot-v13\' found."; \n\
    rm -rf /home/container/node_modules/discord.js-selfbot-v13; \n\
    echo "Selfbot package folder removed."; \n\
else \n\
    echo "No selfbot found."; \n\
fi' > /home/container/check-selfbot.sh \
    && chmod +x /home/container/check-selfbot.sh

CMD ["/bin/bash", "-c", "/home/container/check-selfbot.sh && npm install npm@latest typescript ts-node @types/node --location=global"]

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL  SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh

ENTRYPOINT  ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
