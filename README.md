# IFA Project

Please see requirements.txt and package.json for full details.

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

All services expect to read env variables from .env.dev file. Please make sure you review the example and change the name to env.dev. AND that your gitignore handles env files before you commit super secret stuff to a public repo.

---

### Additional Configuration Notes and Descriptions

#### Program Overview

IFA (Independent Football Australia) is a comprehensive football management platform that streamlines administrative and operational tasks in a football organization. The application supports player registration, team coordination, scheduling of matches, and financial operations such as payment processing. It provides both frontend and backend services, containerized using Docker to ensure consistent environments and seamless local development. The backend is built with Django and PostgreSQL, while the frontend uses a React/Vite stack, and NGINX is used to handle CORS issues during local development.

### Copy Config Files

After cloning the project, make sure to copy the environment example files to their proper names before running the containers.

#### `.env.dev`

This environment file stores all essential variables for the backend services. Update it to reflect your local or staging configuration.

- **docker-compose.yml**: Adjust ports, service names, and Docker container names as needed to prevent conflicts or to align with naming conventions.
- **REACT_APP_API_URL**: This specifies the frontend’s API endpoint, typically the public-facing React app URL, e.g., `http://ifa-stg.ethosbytes.com.au`.
- **SQL_HOST**: Hostname of the PostgreSQL container, e.g., `dev-ifa-db`. This must match the service name in your Docker Compose.
- **APP_ENV**: Application environment identifier, useful for naming/logging, e.g., `<username>-ifa`.

#### `frontend/.env`

Environment settings specific to the frontend (React app). These are injected at build time using Vite.

- **VITE_API_BASE_URL**: The URL used by the frontend to interact with the backend API, e.g., `http://ifa-be.ethosbytes.com.au`.
- **VITE_FRONTEND_BASE_URL**: The public URL where the React frontend is served, e.g., `http://ifa-stg.ethosbytes.com.au`.

#### Optional Configuration

- **nginx.conf**: If you're modifying container names or structure, update this file to reflect the correct proxy paths and container references. Make sure to update proxy container names (e.g., <yourname>-ifa-django / <yourname>-ifa-react) to match your Docker Compose setup.

> ⚠️ **Important:**
> DO NOT PUSH changes to the following files:
>
> - `docker-compose.yml`
> - `nginx.conf`

---

```sh

$> cp .env-example .env.dev
$> cp frontend/.env-example frontend/.env

```

Build containers. Add -up flag to bring services up after build.

```sh

$> docker-compose build

```

Bring containers up. Add -d flag to run output detached from current shell.

```sh

$> docker-compose up -d

```

Bring containers down. Add -v flag to also delete named volumes

```sh

$> docker-compose down

```

View logs by container name.

```sh

$> docker logs <container_name>

```

Enter shell for specified container (must be running)

```sh

$> docker exec -it <container-name> sh

```

### Default Containers, Services and Ports

| Container      | Service | Host Port | Docker Port |
| -------------- | ------- | --------- | ----------- |
| dev-ifa-django | django  | 8000      | 8000        |
| dev-ifa-db     | db      | 5432      | 5432        |
| dev-ifa-react  | react   | 3000      | 3000        |
| dev-ifa-nginx  | nginx   | 80        | 80          |

### Why NGINX for local dev?

Cross-Origin Resource Sharing(CORS) issues will make your browser sad when you serve your site from different ports as we do with this architecture. Using NGINX to proxy requests/responses to/from the correct container/service/ports helps make your browser happy. And it simulates real world infrastructure as a bonus. So...

Please make all requests from your browser through <http://localhost:8080> and NGINX will happily redirect the request and proxy all your services so your browser thinks it's all one and the same protocol/domain/port == CORS bliss.

### Can this be used for production?

This project is focused on making it easier to perform local full stack development. However, it is possible to add new docker compose and docker files to also support production. It's just out of scope for this project. Please have a look in the archives folder for some old production docker files to give you an idea of what worked in the past.
