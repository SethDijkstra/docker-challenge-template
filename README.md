# Docker Challenge Template

This template guides you through the steps for completing two Docker challenges: serving static pages with NGINX and containerizing a NodeJS application.

## CHALLENGE ONE

Serve static pages using Docker and NGINX.

### Steps

1. **Prepare the HTML file**  
   Create an `index.html` file in the `public` folder within the `challenge1` directory. This file should display your name and ID.

2. **Create a Dockerfile**  
   In the root directory of the `challenge1` folder, create a `Dockerfile`.

3. **Specify the NGINX Image**  
   In your `Dockerfile`, use the NGINX image from Docker Hub.

    ```Dockerfile
    FROM nginx:alpine
    ```

4. **Copy Static Content**  
   Copy the contents of the `public` folder to the directory where NGINX serves its pages. Add the following line to your `Dockerfile`:

    ```Dockerfile
    COPY public /usr/share/nginx/html
    ```

5. **Expose Port 80**  
   Expose port 80 in your `Dockerfile` by adding:

    ```Dockerfile
    EXPOSE 80
    ```

6. **Start NGINX**  
   Ensure NGINX stays running in the foreground by adding:

    ```Dockerfile
    CMD ["nginx", "-g", "daemon off;"]
    ```

7. **Build the Docker Image**  
   Navigate to the folder directory in the terminal and build your Docker image:

    ```bash
    docker build -t webserver-image:v1 .
    ```

    Replace `webserver-image:v1` with your preferred image name.

8. **Run the Docker Container**  
   Run the image as a container:

    ```bash
    docker run -d -p 8080:80 --name my-webserver webserver-image:v1
    ```

    Replace `my-webserver` with your preferred container name.

9. **Verification**  
   Take screenshots of both the site being served and the container running in Docker. Place these screenshots in the folder directory.

## CHALLENGE TWO

Containerize and serve a NodeJS application using Docker and NGINX.

### Steps

1. **Prepare Your Application**  
   Use the `challenge2` folder, which contains `package-lock.json`, `package.json`, and `server.js` for a simple NodeJS application.

2. **Create a Dockerfile**  
   In the `challenge2` directory, create a `Dockerfile` with the following content:

    ```Dockerfile
    FROM node:14
    WORKDIR /app
    COPY package.json package-lock.json /app/
    RUN npm install
    COPY . /app
    EXPOSE 3000
    CMD ["node", "server.js"]
    ```

3. **Docker Compose**  
   Create a `docker-compose.yml` file in the same directory:

    ```yaml
    version: '3'
    services:
      app:
        build: .
        ports:
          - "3000:3000"
      nginx:
        image: nginx:alpine
        volumes:
          - ./nginx.conf:/etc/nginx/nginx.conf
        ports:
          - "8000:80"
        depends_on:
          - app
    ```

4. **Configure NGINX**  
   Create an `nginx.conf` file for NGINX to listen on port 8000 and forward requests to the NodeJS application:

    ```nginx
    events {}
    http {
        server {
            listen 80;
            location /api/ {
                proxy_pass http://app:3000/api/;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
            }
        }
    }
    ```

5. **Build and Run Containers**  
   In the `challenge2` directory, build and run the Docker containers:

    ```bash
    docker-compose up --build
    ```

6. **Access the Application**  
   Open a browser and navigate to "http://localhost:8000/api/books" to view a JSON message with all books.
   Then open a browser and navigate to "http://localhost:8000/api/books/1" to view a JSON message with the first book.

7. **Verification**  
   Take screenshots of the running server in Docker, the page being served, and the Docker Compose logs for submission.
