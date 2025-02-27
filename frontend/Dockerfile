# Stage 1: Build the application
FROM node:18-alpine AS build
WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy application source
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the application
FROM node:18-alpine
WORKDIR /app

# Install a lightweight static file server
RUN npm install -g serve

# Copy built files from the build stage
COPY --from=build /app/dist /app

# Set environment variables (optional)
ENV PORT=80
ENV AUTH_API_ADDRESS=${AUTH_API_ADDRESS}
ENV TODOS_API_ADDRESS=${TODOS_API_ADDRESS}

# Expose the port Traefik will route to
EXPOSE 80

# Serve the built application
CMD ["serve", "-s", ".", "-l", "80"]