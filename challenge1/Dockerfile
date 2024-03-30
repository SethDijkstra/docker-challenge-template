# Use the NGinx image from Docker Hub as a base
FROM nginx:alpine
# Copy the contents of the public folder to the directory where NGinx serves files
COPY ./public /usr/share/nginx/html
# Expose port 80 for the web server (NGinx default)
EXPOSE 80
