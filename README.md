# IFA Project

Please see `requirements.txt` and `package.json` for full details.

---

## Technologies Used

### Backend

- Django v5
- Django REST Framework
- Django REST Framework Simple JWT
- PyTest

### Database

- PostgreSQL (Docker v16.1 Alpine Image)

---

## Application Overview

IFA (Independent Football Australia) is a full-stack application designed to manage football operations, including player registration, match scheduling, and payment integration. It uses a modern tech stack with Django and PostgreSQL on the backend and a React-based frontend, containerized with Docker for consistent local development environments.

---

### Edit And Rename .env-example

All services expect to read env variables from `.env.dev` file. Please make sure you review the example and rename it to `.env.dev`. Ensure your `.gitignore` handles env files before committing sensitive data to a public repo.

---

### Additional Configuration Notes and Descriptions

#### Program Overview

IFA (Independent Football Australia) is a comprehensive football management platform that streamlines administrative and operational tasks in a football organization. The application supports player registration, team coordination, match scheduling, and financial operations such as payment processing. It provides both frontend and backend services, containerized using Docker for consistent environments and seamless local development. The backend is built with Django and PostgreSQL, while the frontend uses a React/Vite stack, and NGINX is used to handle CORS issues during local development.

---

### 📁 SFTP Configuration

If you're using **SFTP** to manage or deploy files (e.g., via the **SFTP extension in VS Code**), you can use the following configuration snippet as a reference.

> 🛠 **Important:**
> Replace all placeholder values (`your-name`, `your.server.ip.address`, etc.) with your actual project and server details.

<details>
<summary><strong>Sample <code>sftp.json</code> Configuration</strong></summary>

```json
{
  "name": "ifa-sftp",
  "host": "your.server.ip.address",
  "protocol": "sftp",
  "port": 22,
  "username": "your-username",
  "remotePath": "/home/your-username/IFA-Project/ifa-web",
  "uploadOnSave": true,
  "useTempFile": false,
  "openSsh": false,
  "privateKeyPath": "C:\\Path\\To\\Your\\PrivateKey"
}
```

</details>

#### ✅ Tips for Usage

- **Install the SFTP extension** in your code editor (e.g., VS Code).
- Save the configuration as `.vscode/sftp.json` or where your SFTP tool expects it.
- Ensure your **remotePath** matches the actual deployment directory on your server.
- If you’re using key-pair authentication, make sure `privateKeyPath` points to your local private key.

---

### Copy Config Files

After cloning the project, copy the environment example files to their proper names before running the containers.

#### `.env.dev`

This environment file stores all essential variables for the backend services. Update it for your local or staging configuration.

- **docker-compose.yml**: Adjust ports, service names, and Docker container names as needed.
- **REACT_APP_API_URL**: The frontend’s API endpoint, e.g., `http://ifa-stg.ethosbytes.com.au`.
- **SQL_HOST**: PostgreSQL container hostname, e.g., `dev-ifa-db`.
- **APP_ENV**: Application environment identifier, e.g., `<username>-ifa`.

#### `frontend/.env`

Frontend-specific (React/Vite) environment settings injected at build time:

- **VITE_API_BASE_URL**: Backend API URL, e.g., `http://ifa-be.ethosbytes.com.au`.
- **VITE_FRONTEND_BASE_URL**: Public frontend URL, e.g., `http://ifa-stg.ethosbytes.com.au`.

#### Optional Configuration

- **nginx.conf**: Update this if container names or structure are changed. Match proxy paths and service names to your Docker Compose setup.

> ⚠️ **Important:**
> DO NOT PUSH changes to:
>
> - `docker-compose.yml`
> - `nginx.conf`

---

```sh
$ cp .env-example .env.dev
$ cp frontend/.env-example frontend/.env
```

Build containers (add `-up` flag to bring services up after build):

```sh
$ docker-compose build
```

Bring containers up (add `-d` to run detached):

```sh
$ docker-compose up -d
```

Bring containers down (add `-v` to also delete volumes):

```sh
$ docker-compose down -v
```

View logs by container:

```sh
$ docker logs <container_name>
```

Enter container shell (must be running):

```sh
$ docker exec -it <container-name> sh
```

---

### Default Containers, Services and Ports

| Container      | Service | Host Port | Docker Port |
| -------------- | ------- | --------- | ----------- |
| dev-ifa-django | django  | 8000      | 8000        |
| dev-ifa-db     | db      | 5432      | 5432        |
| dev-ifa-react  | react   | 3000      | 3000        |
| dev-ifa-nginx  | nginx   | 80        | 80          |

---

### Why NGINX for Local Dev?

Cross-Origin Resource Sharing(CORS) issues will make your browser sad when you serve your site from different ports as we do with this architecture. Using NGINX to proxy requests/responses to/from the correct container/service/ports helps make your browser happy. And it simulates real world infrastructure as a bonus. Please make all requests from your browser through

```
http://localhost:8080
```

and NGINX will happily redirect the request and proxy all your services so your browser thinks it's all one and the same protocol/domain/port == CORS bliss.

---

### Can This Be Used for Production?

This project is focused on making it easier to perform local full stack development. However, it is possible to add new docker compose and docker files to also support production. It's just out of scope for this project. Please have a look in the archives folder for some old production docker files to give you an idea of what worked in the past.
