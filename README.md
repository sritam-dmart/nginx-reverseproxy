# Nginx Reverse Proxy with Docker Compose

## Overview

This repository contains a Docker Compose setup that utilizes an Nginx reverse proxy to manage multiple services (Server A, Server B, and Server C) running in separate containers. The reverse proxy routes incoming requests to the appropriate server based on the URL path.

## Key Components

1. **Docker Compose Version**: This configuration uses version 3 of Docker Compose, which allows the definition of multiple services and networks in a YAML format.

2. **Services**:
   - **Server A, B, and C**: Each service runs an Nginx container that serves static files from a specified directory on the host machine. They are configured to listen on the same network (`webnet`) but on different paths when accessed via the reverse proxy.
   - **Nginx Reverse Proxy**: A separate Nginx service acts as a reverse proxy, exposing port 80 to the host. It directs incoming requests to the appropriate server based on the request path.

3. **Volumes**: Each server service mounts a local directory (e.g., `./server_a`, `./server_b`, `./server_c`) to the Nginx HTML directory inside the container. This setup allows for easy management of static content that will be served by the respective servers.

4. **Networking**: All services are connected to the same Docker network (`webnet`), enabling communication between the containers. This allows the reverse proxy to route requests directly to the corresponding server containers.

## Nginx Configuration

The Nginx configuration file (`nginx.conf`) defines how the reverse proxy behaves:

- **Listening Port**: The Nginx server listens on port 80 for incoming HTTP requests.
  
- **Location Blocks**: For each service, there is a location block defined:
  - **/a**: Requests to this path are proxied to `server_a`. The URL path prefix (`/a`) is stripped before forwarding the request to the backend service.
  - **/b**: Similar to `/a`, requests to this path are forwarded to `server_b`, with the prefix stripped.
  - **/c**: Likewise, requests to `/c` are directed to `server_c`, also with the prefix removed.

## Accessing the Services

Once the Docker Compose setup is running, you can access the services using the following URLs:

- **Server A**: [http://localhost/a](http://localhost/a)
- **Server B**: [http://localhost/b](http://localhost/b)
- **Server C**: [http://localhost/c](http://localhost/c)

## Benefits of Using a Reverse Proxy

1. **Path-based Routing**: The reverse proxy allows for path-based routing, enabling multiple services to be accessed through a single entry point (the reverse proxy), improving organization and resource management.

2. **Load Balancing**: While this setup is for static content, a reverse proxy can be configured to load balance between multiple instances of a service if needed.

3. **Security**: By hiding the backend services, the reverse proxy can enhance security. It can also be configured to handle SSL termination, protecting internal services from direct exposure to the internet.

4. **Centralized Configuration**: Managing routing rules and configurations in a single place simplifies deployment and maintenance.

## Summary

This Docker Compose configuration provides a robust way to run multiple Nginx services, serving static content and managed via an Nginx reverse proxy. It allows for flexible routing of requests based on URL paths while simplifying the architecture by providing a single entry point for client requests.
