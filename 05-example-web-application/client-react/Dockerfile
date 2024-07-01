FROM node:lts-alpine3.19 as build
WORKDIR /usr/src/app

COPY package*.json ./

# Use cache mount to speed up install of existing dependencies
RUN --mount=type=cache,target=/usr/src/app/.npm \
    npm set cache /usr/src/app/.npm &&  \
    npm ci 

COPY . .

RUN npm run build

### Using nginx without root privileges
FROM nginxinc/nginx-unprivileged:alpine3.19-perl

COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=build /usr/src/app/dist /usr/share/nginx/html

EXPOSE 8080
