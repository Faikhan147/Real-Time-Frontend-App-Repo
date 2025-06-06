# Step 1: Build  App
FROM node:20-alpine3.21 AS builder
WORKDIR /app
RUN apk update && apk add --no-cache libxml2 && apk upgrade libxml2
COPY package*.json ./
RUN npm install --legacy-peer-deps
COPY . .
RUN npm run build

# Step 2: Serve with NGINX
FROM nginx:alpine
RUN apk add --no-cache curl
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /app/build /usr/share/nginx/html
RUN echo "OK" > /usr/share/nginx/html/healthz
HEALTHCHECK --interval=10s --timeout=3s --retries=3 \
CMD curl --fail http://localhost/healthz || exit 1
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
