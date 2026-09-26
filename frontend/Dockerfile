FROM node:24-alpine AS test-stage

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci

COPY . .

RUN npm test


FROM test-stage AS build-stage

ENV VITE_BACKEND_URL=/api

RUN npm run build

FROM nginx:1.29-alpine AS production-stage

COPY --from=build-stage /usr/src/app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]