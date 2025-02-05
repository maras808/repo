<details>
<summary><b>Navigation</b></summary>

- [About me](#-about-me)
  - [Work experience](#-work-experience)
  - [Interests and hobbies](#-interests-and-hobbies)
  - [Currently learning](#-currently-learning)
- [Technology stack](#-technology-stack)
  - [Software development](#-software-development)
  - [Self-hosting](#%EF%B8%8F-self-hosting)
- [About the repositories](#%EF%B8%8F-about-the-repositories)
</details>

<br>

# `Getting started`

This monorepository is designed to leverage Docker to its full potential, optimizing both development and production workflows. The following documentation provides a step-by-step guide on how to set up, build, and run the project efficiently.

## Prerequisites

To use this repository effectively, ensure you have the following technologies installed:

- A Node.js package manager (e.g., npm, yarn, or pnpm) - Required to run scripts. This repository is designed to be technology-agnostic, meaning any package manager should work. However, the repository scripts have been tested with pnpm, as it provides efficient dependency management. The actual logic and dependency installations occur inside Docker containers, which have their own package manager (pnpm).
- Docker - with support for syntax=docker/dockerfile:1.7-labs directive, containerd snapshotter storage drivers and buildkit.
- Docker Compose - Required as a plugin (docker compose NOT docker-compose). The legacy docker-compose command is EOL and lacks support for key features used in this repository.

---

# `Repository structure`

This is the main repository structure. Structure below is the <b>mandatory</b> elemments of the repo.

<pre>
<a>apps</a>
  <a>app</a>
<a>deployments</a>
  <a>docker-compose.dev.yaml</a>
  <a>docker-compose.prod.yaml</a>
  <a>dockerfiles</a>
    <a>app</a>
      <a>Dockerfile.devshell</a>
      <a>Dockerfile.prodrunner</a>
<a>packages</a>
  <a>package</a>
    <a>dist</a>
    <a>src</a>
<a>package.json</a>
<a>.dockerignore</a>
<a>.gitignore</a>
<a>docker-images.tar</a>
<a>pnpm-lock.yaml</a>
<a>pnpm-workspace.yaml</a>
<a>README.md</a>
</pre>

## Directories

### apps/

Contains all applications in the monorepo, e.g: frontend, backend, and database services.

### apps/app/

The primary development directory where most coding takes place.

### deployments/

Defines configurations for both development and production environments.

### deployments/dockerfiles/

Contains Dockerfiles for each app in the repository.

### deployments/dockerfiles/app/

Each app has its own Dockerfile.devshell and Dockerfile.prodrunner for setting up development and production environments.

### packages/

Stores shared packages that can be used across multiple applications.

### packages/package/

Each package has its own directory within packages/, facilitating code reuse.

### packages/package/dist/

Contains the compiled output of a package after building.

### packages/package/src/

Holds the source code of a package before compilation.

## Files

### Dockerfile.devshell

Defines a development shell container, with volume mounts configured in docker-compose.dev.yaml.

### Dockerfile.prodrunner

Defines a production container, which builds the application into a Docker image instead of using mounted volumes, making it deployable anywhere.

### docker-compose.dev.yaml

The main entry point for running the project in development mode. It runs all apps using their respective Dockerfile.devshell and mounts volumes.

### docker-compose.prod.yaml

Runs all applications in production mode using their Dockerfile.prodrunner.

### .dockerignore

Specifies files and directories that should be ignored by Docker when building images.

### .gitignore

Specifies files and directories that should be ignored by Git in version control.

### docker-images.tar

Contains exported production-ready Docker images. Useful for CI/CD pipelines and deployment automation.

### pnpm-lock.yaml

Locks package versions to ensure consistent dependency resolution across environments. <b>This file is probably in your root directory because a script was run outside it's respective docker container. Same with root node_modules</b>

### pnpm-workspace.yaml

Defines the monorepo structure and manages shared dependencies across multiple packages.

### README.md

This documentation file explaining the structure and usage of the repository.

### package.json

Defines scripts and dependencies for managing the project.

#### Development scripts

- dev - Starts the development environment in the foreground. Logs are shown in the terminal.
- dev:start - Starts the development environment in detached mode (running in the background).
- dev:stop - Stops all running containers without removing them.
- dev:down - Stops and removes all containers, networks, and volumes.

#### Production Scripts

- prod - Starts the production environment in the foreground, rebuilding images if necessary.
- prod:start - Starts the production environment in detached mode, rebuilding images if necessary.
- prod:stop - Stops running containers without removing them.
- prod:down - Stops and removes all containers, networks, and volumes.

#### Build & Export Scripts

- build - Builds production images without running them.
- export:detatched - Saves built production images as a docker-images.tar file for deployment or distribution.
- export:current - Builds the production images and exports them to a docker-images.tar file.
