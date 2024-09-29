# Use the Node.js image for the application
FROM node:18-slim

# Install curl and tini for health check and process management
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl tini && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory inside the container
WORKDIR /usr/local/app

# Install nodemon for file watching in local development
RUN npm install -g nodemon

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci && \
    npm cache clean --force && \
    mv /usr/local/app/node_modules /node_modules

# Copy all files to the container
COPY . .

# Set environment variable and expose port
ENV PORT 80
EXPOSE 80

# Use tini as the entry point and run the application
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["node", "server.js"]
