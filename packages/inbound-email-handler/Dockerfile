FROM node:18.16-alpine@sha256:d5b2a7869a4016b1847986ea52098fa404421e44281bb7615a9e3615e07f37fb

# Run everything after as non-privileged user.
WORKDIR /app

COPY package.json .
COPY yarn.lock .
COPY tsconfig.json .
COPY .prettierrc .
COPY .eslintrc .

COPY /packages/inbound-email-handler/package.json ./packages/inbound-email-handler/package.json
COPY /packages/content-handler/package.json ./packages/content-handler/package.json

RUN yarn install --pure-lockfile

ADD /packages/inbound-email-handler ./packages/inbound-email-handler
ADD /packages/content-handler ./packages/content-handler
RUN yarn workspace @omnivore/content-handler build
RUN yarn workspace @omnivore/inbound-email-handler build

# After building, fetch the production dependencies
RUN rm -rf /app/packages/inbound-email-handler/node_modules
RUN rm -rf /app/node_modules
RUN yarn install --pure-lockfile --production

EXPOSE 8080

CMD ["yarn", "workspace", "@omnivore/inbound-email-handler", "start"]

