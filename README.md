# vue-docker

## Create Project

```
mkdir vue-docker && cd "$_" && docker run --rm -v "${PWD}:/$(basename `pwd`)" -w "/$(basename `pwd`)" -it node:17-alpine3.14 sh -c "yarn global add @vue/cli && vue create ."
```

`Dockerfile` content:

```
# develop stage
FROM node:16.14.0 as develop-stage
WORKDIR /app
COPY package*.json ./
RUN yarn install
COPY . .
# build stage
FROM develop-stage as build-stage
RUN yarn build
# production stage
FROM nginx:1.15.7-alpine as production-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Build Image

```
docker build -t vue-docker .
```

## Run Container

```
docker run -it -p 80:80 --rm vue-docker
```

--

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Lints and fixes files

```
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

### Source

See [Vue with Docker; initialize, develop and build](https://medium.com/@jwdobken/vue-with-docker-initialize-develop-and-build-51fad21ad5e6)
