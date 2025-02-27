# Stage 1: Build the application
FROM node:12-alpine AS build
WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy application source
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the application using Nginx
FROM nginx:alpine

# Copy built application to Nginx's web root
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Set environment variables
ENV PORT=${PORT}
ENV AUTH_API_ADDRESS=${AUTH_API_ADDRESS}
ENV TODOS_API_ADDRESS=${TODOS_API_ADDRESS}

# Expose application port
EXPOSE $PORT

CMD ["nginx", "-g", "daemon off;"]