FROM node:20-alpine

# Set working directory
WORKDIR /usr/src/app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy server source
COPY . .

# Expose backend port
EXPOSE 3000

# Start backend server
CMD ["npm", "start"]
