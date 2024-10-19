FROM --platform=$TARGETOS/$TARGETARCH node:21-bookworm-slim

LABEL author="Embotic" maintainer="luke@embotic.xyz"

RUN apt update \
    && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev \
       ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping \
       libnss3 tini \
    && useradd -m -d /home/container container \
    && rm -rf /usr/bin/dd \
    && rm -rf /usr/bin/fallocate \
    && rm -rf /usr/bin/truncate \
    && rm -rf /usr/bin/xfs_mkfile

RUN echo '#!/bin/bash' > /home/container/block-selfbot.sh && \
    echo 'if [ -d "/home/container/node_modules/discord.js-selfbot-v13" ]; then' >> /home/container/block-selfbot.sh && \
    echo '    echo "Selfbot package folder '\''discord.js-selfbot-v13'\'' found.";' >> /home/container/block-selfbot.sh && \
    echo '    rm -rf /home/container/node_modules/discord.js-selfbot-v13;' >> /home/container/block-selfbot.sh && \
    echo '    echo "Selfbot package folder removed.";' >> /home/container/block-selfbot.sh && \
    echo 'else' >> /home/container/block-selfbot.sh && \
    echo '    echo "No selfbot package folder found."' >> /home/container/block-selfbot.sh && \
    echo 'fi' >> /home/container/block-selfbot.sh

RUN chmod +x /home/container/block-selfbot.sh
RUN /home/container/block-selfbot.sh

RUN echo 'postinstall=/usr/local/bin/block-selfbot.sh' > /home/container/.npmrc
RUN npm install npm@latest typescript ts-node @types/node --location=global

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
