FROM node:18.16-alpine@sha256:d5b2a7869a4016b1847986ea52098fa404421e44281bb7615a9e3615e07f37fb

WORKDIR /app

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true
RUN apk add g++ make python3

ENV PORT 8080

COPY package.json .
COPY yarn.lock .
COPY tsconfig.json .
COPY .eslintrc .

COPY /packages/readabilityjs/package.json ./packages/readabilityjs/package.json
COPY /packages/import-handler/package.json ./packages/import-handler/package.json

RUN yarn install --pure-lockfile

ADD /packages/import-handler ./packages/import-handler
ADD /packages/readabilityjs ./packages/readabilityjs
RUN yarn workspace @omnivore/import-handler build

# After building, fetch the production dependencies
RUN rm -rf /app/packages/import-handler/node_modules
RUN rm -rf /app/node_modules
RUN yarn install --pure-lockfile --production

EXPOSE 8080

ENTRYPOINT ["yarn", "workspace", "@omnivore/import-handler", "start"]
