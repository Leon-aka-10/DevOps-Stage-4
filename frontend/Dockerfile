# Stage 1: Build the application
FROM node:18-alpine AS build
WORKDIR /app

# Install dependencies separately to leverage Docker caching
COPY package.json package-lock.json ./
RUN npm ci --only=production

# Uninstall node-sass and install sass
RUN npm uninstall node-sass && npm install sass

# Upgrade npm globally
RUN npm install -g npm@latest

# Copy application source and build
COPY . .
RUN npm run build

# Clean up unnecessary files to reduce final image size
RUN rm -rf package-lock.json node_modules

# Stage 2: Serve the application
FROM node:18-alpine
WORKDIR /app

# Switch back to root temporarily to install global package
USER root

# Install a lightweight static file server with proper permissions
RUN npm install -g serve --unsafe-perm

# Switch to a non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Copy only the necessary built files from the build stage
COPY --from=build /app/dist /app

# Set environment variables
ENV PORT=80 \
    AUTH_API_ADDRESS=${AUTH_API_ADDRESS} \
    TODOS_API_ADDRESS=${TODOS_API_ADDRESS}

# Expose the port for Traefik
EXPOSE 80

# Health check to verify service availability
HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
    CMD wget -qO- http://localhost:80 || exit 1

# Serve the built application
CMD ["serve", "-s", ".", "-l", "80"]