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
FROM node:18.17-alpine

# Set working directory
WORKDIR /app

# Install the 'serve' package globally
RUN npm install -g serve

# Copy the built React app from the previous stage
COPY --from=build /app/build /app/build

# Expose port 3000
EXPOSE 3000

# Start the static file server using 'serve'
CMD ["serve", "-s", "build", "-l", "3000"]
