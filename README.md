Docker Challenge Template
CHALLENGE ONE: Steps to Serve Static Pages with Docker and NGINX
Step 1
Create an index.html file in the public folder within the challenge1 directory. This file displays my name and ID.

Step 2
Create a Dockerfile in the root directory of the challenge1 folder.

Step 3
Use the NGINX image from Docker Hub in the Dockerfile.

Dockerfile
Copy code
FROM nginx:alpine
Step 4
Copy the contents of the public folder to the directory where NGINX serves its pages. Add into the Dockerfile this code after completing step 3:

Dockerfile
Copy code
COPY public /usr/share/nginx/html
Step 5
Expose port 80 in the Dockerfile. Next in the Dockerfile add this code:

Dockerfile
Copy code
EXPOSE 80
Step 6
Start NGINX and keep it running in the foreground. Lastly in the Dockerfile add this code:

Dockerfile
Copy code
CMD ["nginx", "-g", "daemon off;"]
Step 7
Navigate to the folder directory in the terminal.

Step 8
Build the Docker image with the following command (Replace 'webserver-image:v1' with whatever you want to name your image):

bash
Copy code
docker build -t webserver-image:v1 .
Step 9
Run the image with the following command (my-webserver is what I name my container):

bash
Copy code
docker run -d -p 8080:80 --name my-webserver webserver-image:v1
Step 10
Take screenshots of both the site being served and it running in Docker. Place screenshots in the folder directory.

CHALLENGE TWO: Steps to Containerize and Serve a NodeJS Application with Docker and NGINX
Step 1
Start with the challenge2 folder, already containing package-lock.json, package.json, and server.js for our simple NodeJS application.

Step 2
Create a Dockerfile in the challenge2 directory to build our NodeJS application's Docker container. Dockerfile should look like this:

Dockerfile
Copy code
FROM node:14

WORKDIR /app

COPY package.json package-lock.json /app/

RUN npm install

COPY . /app

EXPOSE 3000

CMD ["node", "server.js"]
Step 3
Create a docker-compose.yml file in the same directory to orchestrate the web server and the application.

yaml
Copy code
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
Step 4
Create an nginx.conf file for NGINX to listen on port 8000 and forward requests to the NodeJS application.

nginx
Copy code
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
Step 5
Navigate to the challenge2 directory in the terminal.

Step 6
Enter the command to build and run the Docker containers:

bash
Copy code
docker-compose up --build
Step 7
After confirming all services were running properly, open a browser and point it to "http://localhost:8000/api/books" to see a JSON message with all books.

Step 8
Take screenshots of the running server in Docker, the page being served (http://localhost:8000/api/books), and the Docker Compose logs for submission.