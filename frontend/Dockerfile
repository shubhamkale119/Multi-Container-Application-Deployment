# Official node image 
FROM node:16-alpine

# working directory in the container
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install


# Copy the rest of the application code
COPY . .

# Build the application
RUN yarn build

# Serve the application using a simple web server
RUN npm install -g serve
CMD ["serve", "-s", "build"]


# Expose the frontend port
EXPOSE 3000