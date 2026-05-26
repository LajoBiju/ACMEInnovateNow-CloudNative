# --- Stage 1: Dependency Build ---
FROM node:20-alpine AS builder
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm ci
COPY . .

# --- Stage 2: Secure Production Runtime ---
FROM node:20-alpine
WORKDIR /usr/src/app
# Copy only the necessary package manifests
COPY package*.json ./
# Install only production dependencies to strip development overhead
RUN npm ci --only=production
# Pull the application source code over from the builder stage
COPY --from=builder /usr/src/app ./

# Security Best Practice: Drop root privileges and run as the built-in non-root user
USER node

EXPOSE 5000
CMD ["node", "server.js"]