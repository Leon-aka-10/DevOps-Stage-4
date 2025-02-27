# Development frontend with hot-reload
FROM node:14

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./ 
RUN npm install

# Copy application files
COPY . . 

# Expose the development port
EXPOSE 8080

# Ensure environment variables are loaded properly
CMD ["sh", "-c", "npm run dev"]