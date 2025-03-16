# Build Stage
FROM node:18-alpine AS build

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json separately for better caching
COPY package.json package-lock.json ./

# Install only production dependencies (skip devDependencies)
RUN npm ci --only=production  

# Copy application source code
COPY . .  

# Build the React app
RUN npm run build  

# Final Stage: Use lightweight Nginx to serve the frontend
FROM node:18-alpine

# Remove default Nginx configuration (optional)
RUN rm -rf /etc/nginx/conf.d

# Copy build files from Node.js stage
COPY --from=build /app/build /usr/share/nginx/html  

# Expose port 80
EXPOSE 80  

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
