# Stage 1: Build React application
FROM node:20-alpine AS builder

# Use /app as the build directory
WORKDIR /app

# Copy dependency files first for Docker caching
COPY package*.json ./

# Install exact dependencies from package-lock.json
RUN npm ci

# Copy the React source code
COPY . .

# Generate optimized production files
RUN npm run build


# Stage 2: Serve React files using Nginx
FROM nginx:alpine

# Copy production files from the builder stage
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Nginx serves the application on port 80
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
