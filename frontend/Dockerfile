# ─── Stage 1: Build Angular application ─────────────────────────
FROM node:18-alpine AS build
WORKDIR /app

# Install dependencies first (layer caching)
COPY package.json package-lock.json ./
RUN npm ci

# Copy source and build for production
COPY . .
# Build command from package.json: "build": "ng build"
# Default configuration is "production" (from angular.json)
RUN npm run build

# ─── Stage 2: Serve with Nginx ──────────────────────────────────
FROM nginx:1.25-alpine

# Remove default Nginx config
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom Nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built Angular app from Stage 1
# Output path from angular.json: "outputPath": "dist/angular-15-crud"
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
