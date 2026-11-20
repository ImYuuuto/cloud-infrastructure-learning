# Month 03 — Docker & Containerization

## Week 1 — Containerization Fundamentals

- Virtual Machines vs Containers
- What is containerization?
- Benefits and limitations of containers
- Docker architecture
- Docker Engine
- Docker Client
- Docker Daemon
- Docker Registry
- Docker Hub
- Images vs Containers
- Container lifecycle
- Basic Docker commands


---

## Week 2 — Docker Images and Dockerfiles

- Docker image layers
- Image caching
- Image tags and versions
- Pulling and pushing images
- Understanding Dockerfiles
- Dockerfile instructions:
  - `FROM`
  - `WORKDIR`
  - `COPY`
  - `ADD`
  - `RUN`
  - `CMD`
  - `ENTRYPOINT`
  - `EXPOSE`
  - `ENV`
  - `ARG`
  - `USER`
- Build context
- Image optimization
- Multi-stage builds
- `.dockerignore`

## Week 3 — Docker Networking and Storage

### Docker Networking

- Docker network fundamentals
- Bridge networks
- Host networking
- None networking
- Custom bridge networks
- Container-to-container communication
- Port mapping
- DNS resolution between containers
- Network inspection

### Docker Storage

- Container filesystem
- Ephemeral container data
- Volumes
- Bind mounts
- tmpfs mounts
- Volume management
- Persistent data


---

## Week 4 — Docker Compose and Production Fundamentals

- Why Docker Compose?
- Compose YAML structure
- Services
- Networks
- Volumes
- Environment variables
- `.env` files
- Service dependencies
- Health checks
- Restart policies
- Container resource limits
- Docker security fundamentals
- Container troubleshooting
- Image publishing


---

## Final Project — Containerized Full-Stack Application

Containerize a full-stack application using Docker Compose.

**Suggested architecture:**

- Frontend: React
- Backend: Node.js and Express
- Database: MySQL
- Reverse proxy: Nginx (optional extension)

**Requirements:**
- Create a Dockerfile for the frontend.
- Create a Dockerfile for the backend.
- Use an official MySQL image.
- Connect all services through a Docker network.
- Persist database data using volumes.
- Configure environment variables using `.env`.
- Use Docker Compose to manage the entire application.
- Add health checks where appropriate.
- Document how to start, stop, and troubleshoot the application.

**Expected deliverables:**
- Dockerfiles.
- `compose.yaml`.
- `.dockerignore` files.
- Environment variable example file (`.env.example`).
- Project documentation (`README.md`).
- Screenshots or terminal output showing the running containers.
- Explanation of the architecture and container communication.

---

## Month 3 Completion Checklist

- [ ] Understand containerization and Docker architecture.
- [ ] Manage containers and images.
- [ ] Write Dockerfiles independently.
- [ ] Understand image layers and multi-stage builds.
- [ ] Understand Docker networking.
- [ ] Manage volumes and persistent storage.
- [ ] Build multi-container environments using Docker Compose.
- [ ] Configure environment variables and health checks.
- [ ] Understand basic Docker security.
- [ ] Complete the Containerized Full-Stack Application.

**Final goal:** Be capable of containerizing applications, building optimized images, managing persistent data, and deploying multi-container environments using Docker Compose.