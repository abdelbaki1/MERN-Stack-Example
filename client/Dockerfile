# specifing which base image to run-on
FROM node:16-alpine AS build

# specifong the work directory
WORKDIR /app

# we will be copying the dependency file and then install them inside the container

COPY package.json .
COPY yarn.lock .
RUN npm install --production

ADD public /app/public
ADD src /app/src

ARG SERVER_URI

RUN sed -i "s,http://localhost:5001,$SERVER_URI,g" src/buildEnv.js

RUN npm run build

# copying the builed artifact to the nginx base image

FROM nginx:1.21-alpine as runtime

COPY --from=build /app/build /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
# RUN THE NGINX SERVER

CMD ["nginx", "-g", "daemon off;"]
