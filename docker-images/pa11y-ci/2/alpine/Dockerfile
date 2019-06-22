FROM node:12-alpine

# Tell Puppeteer to skip installing Chrome. We'll be using the installed package.
#ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true

#RUN apk add \
#        chromium \
#        fontconfig \
##        font-ipa \
#        font-noto \
#        freetype \
#        harfbuzz \
#        nss \
#        ttf-freefont \
##        wqy-zenhei \
#    && rm -rf /var/cache/apk/*

RUN apk update && apk upgrade && \
    echo @edge http://nl.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories && \
    echo @edge http://nl.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories && \
    apk add --no-cache \
#      chromium@edge=~73.0.3683.103 \
      chromium@edge \
      nss@edge \
      freetype@edge \
      harfbuzz@edge \
      ttf-freefont@edge\
      udev

# Tell Puppeteer to skip installing Chrome. We'll be using the installed package.
#ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true

# Puppeteer v1.12.2 works with Chromium 73.
#RUN yarn add puppeteer@1.12.2

#RUN npm install -g puppeteer@1.12.2 pa11y-ci@2.* depcheck --unsafe-perm \
#RUN npm install -g puppeteer@1.12.2 pa11y-ci@2.* --unsafe-perm \
#RUN npm install -g pa11y-ci@1.* --unsafe-perm \
RUN npm install -g pa11y-ci@2.* depcheck --unsafe-perm \
    && npm cache clean --force

# Add user so we don't need --no-sandbox.
#RUN addgroup -S pptruser && adduser -S -g pptruser pptruser \
#    && mkdir -p /home/pptruser/Downloads /app \
#    && chown -R pptruser:pptruser /home/pptruser \
#    && chown -R pptruser:pptruser /app
#
## Run everything after as non-privileged user.
#USER pptruser

#RUN apt-get update && apt-get install -y curl wget apt-transport-https \
#	&& apt-get install -y
#	  wget
#	  unzip
#	  locales
#	  gconf-service
#	  libasound2
#	  libatk1.0-0
#	  libc6
#	  libcairo2
#	  libcups2
#	  libdbus-1-3
#	  libexpat1
#	  libfontconfig1
#	  libgcc1
#	  libgconf-2-4
#	  libgdk-pixbuf2.0-0
#	  libglib2.0-0
#	  libgtk-3-0
#	  libnspr4
#	  libpango-1.0-0
#	  libpangocairo-1.0-0
#	  libstdc++6
#	  libx11-6
#	  libx11-xcb1
#	  libxcb1
#	  libxcomposite1
#	  libxcursor1
#	  libxdamage1
#	  libxext6
#	  libxfixes3
#	  libxi6
#	  libxrandr2
#	  libxrender1
#	  libxss1
#	  libxtst6
#	  ca-certificates
#	  fonts-liberation
#	  libappindicator1
#	  libnss3
#	  lsb-release
#	  xdg-utils

WORKDIR /workspace
