##  Dockerized Webserver with Firewall

This Docker Compose file defines a two-container application: a firewall and a webserver.

### Project Structure

This file assumes the following structure:

```
.
├── html
│   └── index.html
└── docker-compose.yml 
```

* **`html/index.html`:** Contains the content of your webserver (e.g., a simple HTML page).
* **`docker-compose.yml`:**  Defines the services and their configurations.

### Services

#### `firewall`

* **Image:** `ubuntu:20.04` (Ubuntu 20.04)
* **Command:** Configures `iptables` to block HTTP requests exceeding 20 per minute from the same IP address. This acts as a basic DDoS protection. It also allows HTTPS traffic and sets default policies for INPUT, FORWARD, and OUTPUT chains.
* **Ports:** Exposes ports 80 (HTTP) and 443 (HTTPS) from the container to the host machine.
* **Container Name:** `firewall`
* **Restart Policy:** `unless-stopped` - the container will restart automatically unless manually stopped.

#### `webserver`

* **Image:** `python:3.9` (Python 3.9)
* **Ports:** Exposes port 8080 from the container to the host machine.
* **Volumes:** Mounts the `html` folder from the host machine into the container's `/usr/src/app` directory. This makes the webserver accessible to the container.
* **Container Name:** `webserver`
* **Command:** Starts a simple HTTP server using the Python standard library, serving files from the `/usr/src/app` directory.
* **Restart Policy:** `unless-stopped` - the container will restart automatically unless manually stopped.

### Usage

1. **Create `index.html`:**  Place your webserver content in the `html/index.html` file.
2. **Build and Run:** Execute the following command from the project root directory: `docker-compose up -d` 
3. **Access Webserver:** Open a web browser and navigate to `http://localhost:8080` to access the webserver.

### Considerations

* **Security:** The firewall implementation is basic. For a production environment, consider more robust firewall solutions and security hardening.
* **Webserver:** The simple HTTP server is not suitable for production. Consider using a more robust webserver like Nginx or Apache.
* **DDoS Protection:** The firewall rule is a basic rate-limiting mechanism. Consider more advanced techniques for DDoS protection.
* **Logging:**  Add logging to the firewall and webserver for troubleshooting. 
* **Monitoring:**  Implement monitoring tools to track the performance and resource usage of the containers.

This README provides a basic overview of the project. Further documentation for specific functionalities or additional features can be added as needed.


