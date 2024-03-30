# Use the NGinx image from Docker Hub as a base
FROM nginx:alpine

COPY ./public /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]