# react-frontend/Dockerfile (multi-stage)
# Build stage
FROM node:18-alpine AS build
LABEL authors="hamza-jaa"
WORKDIR /app
# copy package files first for caching
COPY package*.json ./
#RUN npm ci --silent
RUN npm ci --legacy-peer-deps --silent
COPY . .
# build script from your package.json (uses webpack)
RUN npm run build

# Production stage - serve with nginx
FROM nginx:stable-alpine
# remove default nginx content
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/build /usr/share/nginx/html
# optional: add nginx conf if you need SPA routing (rewrite to index.html)
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
