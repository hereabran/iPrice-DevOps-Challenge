# Platform Engineer Challenge

This guide provides step-by-step instructions for deploying the application using Docker Compose. The application code, Dockerfile, and Docker Compose file can be found in the DevOps-Challenge repository.

## Prerequisites
- Docker Engine and Docker Compose are installed on your system.
- Internet connectivity to pull Docker images and dependencies.

### Deployment Steps
1. Clone the repository:
    ```bash
    git clone https://github.com/iPriceGroup/DevOps-Challenge.git
    ```

2. Move to that directory:
    ```bash
    cd DevOps-Challenge
    ```

#### Make the following changes to the code:
1. Modify the Dockerfile:
    ```dockerfile
    COPY ./app/composer.* /var/www/
    ```
    Update the source path to copy from the app and add a wildcard to copy both composer.json and composer.lock if they exist.

    ```dockerfile
        locales \
    ```
    Add the backslash “\” to resolve errors while building the docker image.

    ```dockerfile
    RUN groupadd -g 1000 www && useradd -u 1000 -ms /bin/bash -g www www
    ```
    Grouping 2 lines into one to reduce docker image layers.

    ```dockerfile
    COPY ./app /var/www/
    ```
    Update the source path to copy from the app.

    ```dockerfile
    COPY --chown=www:www ./app /var/www/
    ```
    Update the source path to copy from the app.

    ```dockerfile
    RUN chown -R www:www /var/www/
    ```
    Add the above line to change the ownership of the working directory and all the files before installing dependencies.

    ```dockerfile
    USER www
    ```
      Update the current user from www2 to www.

    ```dockerfile
    RUN composer install
    ```
    Move this line after selecting the current user to resolve waring composer install using root user.

### Modify the docker-compose.yml:
```yaml
  app:
    build:
      context: .
```
Update build context to DevOps-Challenge or current directory, because the Dockerfile is located in the root directory of the task folder.

```yaml
  webserver:
    image: nginx:alpine
```
Update image name to nginx.

```yaml
    networks:
      - app-network
```
Update webserver network to app-network for FastCGI proxy connection to the app.

### Build and run the containers using Docker Compose:
```bash
docker-compose up -build -d
```

#### Access the application:
Open your browser and visit http://localhost.
You should see the application running.

> **Note:** Make sure you have Docker and Docker Compose installed on your machine before following these steps.

## Documentation on what the application is, and how it works.
The application in the DevOps-Challenge repository is a simple PHP web application built with the Laravel framework. It serves as an example of setting up a PHP application with Dockerfile and Docker Compose.

### Functionality:
The application provides a basic simple Hello World HTTP application that can be deployed using Docker Compose. It demonstrates the process of containerizing an application and deploying it Docker Compose.

### How It Works:
#### Backend (PHP and Laravel):
The backend of the application is written in PHP and utilizes the Laravel framework.
Laravel provides a robust and expressive syntax, along with various tools and features, to simplify and accelerate web development.
The backend handles incoming HTTP requests, processes them, and returns the desired response to the client. 
It follows the MVC (Model-View-Controller) architectural pattern, where models represent the data structure, views handle the presentation layer, and controllers manage the application's logic.

#### Frontend (HTML, CSS, and JavaScript):
The frontend of the application is built using HTML, CSS, and JavaScript.
It provides a user-friendly interface for interacting with the application.
The HTML templates define the structure and layout of the web pages.
CSS stylesheets are used for styling and visual enhancements.
JavaScript adds interactivity and dynamic behavior to the frontend components.

#### Docker:
The application is containerized using Docker and Docker Compose.
Docker allows for consistent and reproducible deployment of the application by packaging all the necessary dependencies and configurations into containers.
The Docker Compose file defines the services required for the application: the PHP service and the Nginx web server.
The PHP service runs the PHP-FPM process to handle PHP requests.
The Nginx service acts as a reverse proxy, forwarding incoming HTTP requests to the PHP service and serving static files.

---

Overall, the application demonstrates a typical setup for a PHP web application using the Laravel framework and Docker Compose. It showcases the separation of concerns between the backend and frontend components and highlights the benefits of containerization for application deployment and scalability.