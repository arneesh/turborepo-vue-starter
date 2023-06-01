FROM node:18-alpine AS base

FROM base AS builder
ARG APP_SCOPE
ENV APP_SCOPE ${APP_SCOPE}
# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app
RUN yarn global add turbo
COPY . .
RUN yarn turbo prune --scope=${APP_SCOPE} --docker

# Add lockfile and package.json's of isolated subworkspace
FROM base AS installer
ARG APP_SCOPE
ENV APP_SCOPE ${APP_SCOPE}
RUN apk add --no-cache libc6-compat
RUN apk update
WORKDIR /app
COPY .gitignore .gitignore
COPY --from=builder /app/out/json/ .
COPY --from=builder /app/out/yarn.lock ./yarn.lock
COPY --from=builder /app/out/full/ .
RUN yarn install
COPY turbo.json turbo.json
RUN yarn turbo run build --scope=${APP_SCOPE}

# production stage
FROM nginx:stable-alpine as production-stage
ARG APP_SCOPE
ENV APP_SCOPE ${APP_SCOPE}
COPY --from=installer /app/apps/${APP_SCOPE}/dist /usr/share/nginx/html
EXPOSE 80
STOPSIGNAL SIGTERM
CMD ["nginx", "-g", "daemon off;"]

