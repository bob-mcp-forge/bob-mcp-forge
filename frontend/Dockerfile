# syntax=docker/dockerfile:1
FROM nginx:alpine

# Copy static frontend files
COPY . /usr/share/nginx/html

# nginx listens on port 80 by default
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
