# Step 1: Use a Node image for building and testing. Uses Node.js version 10, which is compatible with Angular 7
FROM node:10 AS build

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies separately
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# Run any defined tests in the project
#RUN npm test

# Build the Angular project for production
RUN npm run build -- --prod

# Step 2: Use an NGINX image to serve the built application
FROM nginx:alpine AS runtime

# Copy the Angular build output from the previous stage to the NGINX directory
COPY --from=build /app/dist/shop /usr/share/nginx/html

# Copy custom NGINX configuration file
COPY nginx/default.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start NGINX server
CMD ["nginx", "-g", "daemon off;"]
