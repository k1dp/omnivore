FROM node:18.16-alpine@sha256:d5b2a7869a4016b1847986ea52098fa404421e44281bb7615a9e3615e07f37fb

# Installs latest Chromium (92) package.
RUN apk add --no-cache \
    chromium \
    nss \
    freetype \
    harfbuzz \
    ca-certificates \
    ttf-freefont \
    nodejs \
    yarn \
    g++ \
    make \
    python3

WORKDIR /app

ENV CHROMIUM_PATH /usr/bin/chromium-browser
ENV LAUNCH_HEADLESS=true

COPY package.json .
COPY yarn.lock .
COPY tsconfig.json .
COPY .prettierrc .
COPY .eslintrc .

COPY /packages/readabilityjs/package.json ./packages/readabilityjs/package.json
COPY /packages/content-handler/package.json ./packages/content-handler/package.json
COPY /packages/puppeteer-parse/package.json ./packages/puppeteer-parse/package.json

RUN yarn install --pure-lockfile

ADD /packages/content-fetch ./packages/content-fetch
ADD /packages/content-handler ./packages/content-handler
ADD /packages/puppeteer-parse ./packages/puppeteer-parse
ADD /packages/readabilityjs ./packages/readabilityjs
RUN yarn workspace @omnivore/content-handler build

# After building, fetch the production dependencies
RUN rm -rf /app/packages/content-fetch/node_modules
RUN rm -rf /app/node_modules
RUN yarn install --pure-lockfile --production

EXPOSE 8080

CMD ["yarn", "workspace", "@omnivore/content-fetch", "start"]

