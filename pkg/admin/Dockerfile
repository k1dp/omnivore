FROM node:14.18-alpine@sha256:a9b200d25469261a4cccde176db08ec9f0b5799fa4220d8edfe57e69684c6dad

WORKDIR /app

COPY package.json .
COPY yarn.lock .

RUN yarn install --frozen-lockfile

COPY . .
RUN NODE_ENV=production yarn build

ENV PORT=8080
EXPOSE 8080

ENV NODE_ENV production

CMD yarn start
