FROM alpine:3.6

# ensure local python is preferred over distribution python
ENV PATH=/usr/local/bin:$PATH LANG=C.UTF-8 LANGUAGE=C.UTF-8 LC_ALL=C.UTF-8 TIMEZONE=America/New_York

# Set Node version
ENV NODE_VERSION=v8.1.3 NODE_PREFIX=/usr/local

# install ca-certificates so that HTTPS works consistently
# and other required deps
RUN apk update \
	&& apk add --no-cache ca-certificates libgcc libstdc++ \
	&& update-ca-certificates

RUN set -ex \
	# Set up deps
	&& apk add --no-cache --virtual .build-deps  \
		g++ \
        gcc \
        gnupg \
        linux-headers \
        make \
        openssl \
        paxctl \
        python \
        tar \
        wget \

    # Download and validate the NodeJS source
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys \
        9554F04D7259F04124DE6B476D5A82AC7E37093B \
        94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
        0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
        FD3A5288F042B6850C66B31F09FE44734EB7990E \
        71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
        DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
        C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
        B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    && mkdir -p /usr/src/node \
    && cd /usr/src/node \
    && wget -O node-${NODE_VERSION}.tar.gz "https://nodejs.org/dist/${NODE_VERSION}/node-${NODE_VERSION}.tar.gz" \
    && wget -O SHASUMS256.txt.asc "https://nodejs.org/dist/${NODE_VERSION}/SHASUMS256.txt.asc" \
    && gpg --verify SHASUMS256.txt.asc \
    && grep node-${NODE_VERSION}.tar.gz SHASUMS256.txt.asc | sha256sum -c - \

    # Compile and install
    && cd /usr/src/node \
    && tar -zxf node-${NODE_VERSION}.tar.gz \
    && rm node-${NODE_VERSION}.tar.gz \
    && cd node-${NODE_VERSION} \
    && export GYP_DEFINES="linux_use_gold_flags=0" \
    && ./configure --prefix=${NODE_PREFIX} \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && make -j${NPROC} -C out mksnapshot BUILDTYPE=Release \
    && paxctl -cm out/Release/mksnapshot \
    && make -j${NPROC} \
    && make install \
    && paxctl -cm ${NODE_PREFIX}/bin/node \
    && rm -rf /usr/src/node \
    && apk del .build-deps

RUN set -ex \
    # Install node packages
    && npm install --silent -g \
        gulp-cli \
        grunt-cli \
        bower \
        yarn

CMD ["/usr/local/bin/gulp"]
