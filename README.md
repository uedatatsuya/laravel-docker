# docker-laravel 🐳

<p align="center">
    <img src="https://user-images.githubusercontent.com/35098175/145682384-0f531ede-96e0-44c3-a35e-32494bd9af42.png" alt="docker-laravel">
</p>
<p align="center">
    <img src="https://github.com/ucan-lab/docker-laravel/actions/workflows/laravel-create-project.yml/badge.svg" alt="Test laravel-create-project.yml">
    <img src="https://github.com/ucan-lab/docker-laravel/actions/workflows/laravel-git-clone.yml/badge.svg" alt="Test laravel-git-clone.yml">
    <img src="https://img.shields.io/github/license/ucan-lab/docker-laravel" alt="License">
</p>

## Introduction

Build a simple laravel development environment with docker-compose. Compatible with Windows(WSL2), macOS(M1) and Linux.

## Usage

### Lightweight setup (latest Laravel, PHP 8.4, SQLite)

For a new application, use the lightweight development configuration:

```bash
make light-install
```

Open http://localhost:8080. This creates the latest compatible stable Laravel
application in `src`, installs its dependencies, initializes SQLite, and starts
the development server. The `src` directory must be empty on first installation.
Application files and the SQLite database persist in `src` when containers stop.

```bash
make light-up      # Start an installed application
make light-down    # Stop and remove the development container
make light-logs    # Follow server logs
make light-test    # Run Laravel's tests
make light-app     # Open a shell
```

The equivalent Compose command is
`docker compose -f docker-compose.lightweight.yml up -d`.
Set `WEB_PUBLISHED_PORT` to change port 8080. On Linux, if your user/group IDs
are not 1000, set `LOCAL_UID=$(id -u)` and `LOCAL_GID=$(id -g)` when running make.

This configuration uses one Alpine PHP container with Composer, SQLite, and
Laravel's development server. It does not provide MySQL, Nginx, Xdebug, or MailHog;
mail is written to Laravel's log. It is intended for local development, not
production. The original configuration and its commands below remain available
through `docker-compose.yml`.

### Laravel install

1. Click [Use this template](https://github.com/ucan-lab/docker-laravel/generate)
2. Git clone & change directory
3. Execute the following command

```bash
$ mkdir -p src
$ docker compose build
$ docker compose up -d
$ docker compose exec app composer create-project --prefer-dist laravel/laravel .
$ docker compose exec app php artisan key:generate
$ docker compose exec app php artisan storage:link
$ docker compose exec app chmod -R 777 storage bootstrap/cache
$ docker compose exec app php artisan migrate
```

http://localhost

### Laravel setup

1. Git clone & change directory
2. Execute the following command

```bash
$ make install
```

http://localhost

## Tips

- Read this [Makefile](https://github.com/ucan-lab/docker-laravel/blob/main/Makefile).
- Read this [Wiki](https://github.com/ucan-lab/docker-laravel/wiki).

## Container structures

```bash
├── app
├── web
└── db
```

### app container

- Base image
  - [php](https://hub.docker.com/_/php):8.1-fpm-bullseye
  - [composer](https://hub.docker.com/_/composer):2.2

### web container

- Base image
  - [nginx](https://hub.docker.com/_/nginx):1.22

### db container

- Base image
  - [mysql/mysql-server](https://hub.docker.com/r/mysql/mysql-server):8.0

### mailhog container

- Base image
  - [mailhog/mailhog](https://hub.docker.com/r/mailhog/mailhog)
