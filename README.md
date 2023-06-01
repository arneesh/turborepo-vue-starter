# Turborepo Vue 3 Starter

This is a turborepo vue 3 starter repository.

<p align="center"><a href="https://vuejs.org" target="_blank" rel="noopener noreferrer"><img width="100" src="https://vuejs.org/images/logo.png" alt="Vue logo"></a></p>

<p align="center"><a href="https://turborepo.org/" target="_blank" rel="noopener noreferrer"><img width="300" src="https://user-images.githubusercontent.com/4060187/106504110-82f58d00-6494-11eb-87b7-a16d4f68bc5a.png" alt="Turborepo logo"></a></p>

## What's inside?

This turborepo uses [Yarn](https://classic.yarnpkg.com/lang/en/) as a package manager. It includes the following packages/apps:

### Apps and Packages

- `docs`: a [Vue.js](https://vuejs.org/) app - `port: 3000 `
- `web`: another [Vue.js](https://vuejs.org/) app - `port: 3001`
- `tsconfig`: `tsconfig.json`s used throughout the monorepo

### Build

To build all apps and packages, run the following command:

```
yarn run build
```

### Develop

To develop all apps and packages, run the following command:

```
yarn run dev
```

### Remote Caching

Turborepo can use a technique known as [Remote Caching (Beta)](https://turborepo.org/docs/features/remote-caching) to share cache artifacts across machines, enabling you to share build caches with your team and CI/CD pipelines.

By default, Turborepo will cache locally. To enable Remote Caching (Beta) you will need an account with Vercel. If you don't have an account you can [create one](https://vercel.com/signup), then enter the following commands:

```
npx turbo login
```

This will authenticate the Turborepo CLI with your [Vercel account](https://vercel.com/docs/concepts/personal-accounts/overview).

Next, you can link your Turborepo to your Remote Cache by running the following command from the root of your turborepo:

```
npx turbo link
```

## Useful Links

Learn more about the power of Turborepo:

- [Pipelines](https://turborepo.org/docs/features/pipelines)
- [Caching](https://turborepo.org/docs/features/caching)
- [Remote Caching (Beta)](https://turborepo.org/docs/features/remote-caching)
- [Scoped Tasks](https://turborepo.org/docs/features/scopes)
- [Configuration Options](https://turborepo.org/docs/reference/configuration)
- [CLI Usage](https://turborepo.org/docs/reference/command-line-reference)

### Docker Support

If you want to deploy with docker there is a Dockerfile included to help with that  
_Note: Dockerfile only works to build docker image to deploy and does not support local development_

#### How to build
Run this command to build a docker image where the {SCOPE_NAME} is the workspace you want to build
```go
docker-compose up {SCOPE_NAME} 
```

For example:
```go
docker-compose up web
```
Brings up web app and
```go
docker-compose up docs
```
Brings up docs app 

if you want to add another app and build a docker image for it, you can just add a new service in docker-compose like
```yaml
  newapp:
    container_name: newapp
    build:
      args:
        APP_SCOPE: "newapp"
      context: .
      dockerfile: ./docker/Dockerfile
    restart: always
    ports:
      - "3003:80"
```

and run 
```go
docker-compose up newapp
```
to bring up newapp app