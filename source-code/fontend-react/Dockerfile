# Build Stage
FROM node:18.17-alpine AS build

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json first for caching
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci --only=production

# Copy the rest of the source code
COPY ./ /app/

# Build the React app
RUN npm run build

# Production Stage
FROM nginx:1.25-alpine

# Copy custom Nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built React app from previous stage
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
