#FROM node:16-alpine AS build
#WORKDIR /app
#
#COPY package*.json ./
#RUN npm ci --legacy-peer-deps
#
#COPY . .
#RUN npm run build
#
#FROM nginx:stable-alpine
#RUN rm -rf /usr/share/nginx/html/*
#COPY --from=build /app/build /usr/share/nginx/html
#COPY nginx.conf /etc/nginx/conf.d/default.conf
#
#EXPOSE 80
#CMD ["nginx", "-g", "daemon off;"]

# Use Node 18 (LTS) instead of 16
FROM node:18-alpine AS build
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies — remove --legacy-peer-deps once deps are clean
RUN npm ci --legacy-peer-deps

# Copy source
COPY . .

# Build the app using react-scripts (CRITICAL FIX)
RUN npm run build

# Production stage
FROM nginx:stable-alpine
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]