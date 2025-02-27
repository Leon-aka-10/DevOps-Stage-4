FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json ./
RUN npm install --only=production

# Copy application source
COPY . .

# Set environment variables
ENV PORT=${TODOS_API_PORT}
ENV REDIS_HOST=${REDIS_HOST}
ENV REDIS_PORT=${REDIS_PORT}
ENV REDIS_CHANNEL=${REDIS_CHANNEL}
ENV JWT_SECRET=${JWT_SECRET}

# Expose application port dynamically
EXPOSE $PORT

# Start the application
CMD ["node", "server.js"]