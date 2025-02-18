This is a public chat app with mock responses generated by file core/mock.py.

### Prerequisites
- Docker and Docker Compose must be installed on your system.
- A basic understanding of Docker, Docker Compose, React, and FastAPI is required.

### Project Structure Overview
- **Frontend**: A React application located in the `/frontend` directory.
- **Backend**: A FastAPI application located inside the `/app` directory.

### Important Files
- `.env`: Contains the environment variables required for the Docker containers.
- `docker-compose.prod.yml`: Docker Compose file used to orchestrate the production deployment.
- `docker-compose.dev.yml`: Docker Compose file used to orchestrate the development deployment.
- `routers/chat.py`: Handles routing of chat information from the front end.
- `main.py`: Incorporates socket.io for real-time communication features.

### Step-by-Step Production Deployment Guide

#### 1. Preparing Environment Variables
- Duplicate the provided `.env` sample. Customize the values to suit your specific environment and needs like the PostgreSQL, TRAEFIK URLs without the `HTTP/HTTPS` just the host name `host.domain.com`
- Generate the Traefik dashboard's hashed password using `echo -n {password} | md5sum`, then paste it to this variable `TRAEFIK_DASHBOARD_PASSWORD`

#### 2. Configuring the Docker Compose File
- Examine the `docker-compose.prod.yml` file. It outlines three services: `Traefik` for reverse proxying, `backend`, and `frontend`.

#### 3. Deploying with Docker Compose
- Open a terminal and navigate to your project's root directory, where your `docker-compose.prod.yml` file is located.
- Execute `docker compose -f docker-compose.prod.yml up -d` to initiate all services specified in the Docker Compose file.

#### 4. Verifying the Deployment
- Once the containers are operational, access the Traefik dashboard to confirm that your services are correctly routed. Utilize the URL defined in your `.env` file under `TRAEFIK_DASHBOARD_URL`.
- Verify the frontend and backend services are accessible and functioning correctly using the URLs specified in the `.env` file (`TRAEFIK_FRONTEND_URL` and `TRAEFIK_BACKEND_URL`).

### Step-by-Step Development Deployment Guide
- Duplicate the provided `.env` sample. Customize the values to suit your specific environment and needs like the PostgreSQL, TRAEFIK URLs without the `HTTP/HTTPS` just the host name `host.domain.com`
- To generate a qualified self-signed certificate for development purposes, use [mkcert](https://github.com/FiloSottile/mkcert). **Note**: This tool must be installed on your host machine for the browser to recognize the certificate as valid.
- Please note self-signed certificates must be named as `local-cert.pem`and `local-key.pem` and must be located under certs before executing the docker compose command. You can execute this command from the project root file and update the hostname if needed`mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "domain.local" "*.domain.local"`
- For accessing your services via custom domains like `traefik.domain.local`, `backend.domain.local`, and `frontend.domain.local` without the `HTTP/HTTPS` in other words the hostname only, you may need to edit your system's hosts file to resolve these domains to your local machine (127.0.0.1).
- For development, please use `docker compose -f docker-compose.dev.yml up -d`

### Additional Notes
- Regularly check for updates to dependencies and Docker images to ensure your application remains secure and performant.
- Monitor application logs and performance to proactively address any issues or bottlenecks.
- Please note in case you are using localhost subdomain the certificate must be generated with an explict hostnames rather than wildcard, because the browsers do not support it i.e. `mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "localhost" "backend.localhost"`
This guide aims to facilitate a smooth deployment process, from initial setup to running a fully functional public chat application in both development and production environments.
