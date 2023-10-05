FROM node:18.16-alpine@sha256:d5b2a7869a4016b1847986ea52098fa404421e44281bb7615a9e3615e07f37fb

# Run everything after as non-privileged user.
WORKDIR /app

COPY package.json .
COPY yarn.lock .
COPY tsconfig.json .
COPY .prettierrc .
COPY .eslintrc .

COPY /packages/pdf-handler/package.json ./packages/pdf-handler/package.json

RUN yarn install --pure-lockfile

ADD /packages/pdf-handler ./packages/pdf-handler
RUN yarn workspace @omnivore/pdf-handler build

# After building, fetch the production dependencies
RUN rm -rf /app/packages/pdf-handler/node_modules
RUN rm -rf /app/node_modules
RUN yarn install --pure-lockfile --production

EXPOSE 8080

CMD ["yarn", "workspace", "@omnivore/pdf-handler", "start"]

