# Build Stage
FROM node:20-alpine AS build

# Set working directory
WORKDIR /usr/src/app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Create env file
RUN cp .env.example .env

# Build frontend
RUN npm run build

# Production Stage
FROM nginx:alpine

# Copy build output
COPY --from=build /usr/src/app/dist /usr/share/nginx/html

# Copy nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port
EXPOSE 8080

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
