# Stage 1: Install dependencies
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json ./
RUN npm install --production

# Stage 2: Production image
FROM node:18-alpine
WORKDIR /app

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy dependencies and source code
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# Set ownership
RUN chown -R appuser:appgroup /app
USER appuser

# Backend runs on port 8080 (from server.js: process.env.PORT || 8080)
EXPOSE 8080

# Entry point: server.js (from package.json "main" field; no "start" script defined)
CMD ["node", "server.js"]
