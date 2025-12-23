FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./

# Install dependencies with legacy peer deps
RUN npm install --legacy-peer-deps

# CRITICAL: Remove incompatible @types/node and install compatible version
RUN npm uninstall @types/node
RUN npm install --save-dev @types/node@18.19.50 --legacy-peer-deps

COPY . .

# Build the application
RUN npm run build -- --configuration production

FROM nginx:alpine

# Copy built Angular app
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html/

# Copy nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
