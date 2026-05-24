Current VPS setup:

Provider:

* DigitalOcean

OS:

* Ubuntu 24.04

Server Type:

* Single VPS
* Multiple Next.js apps will run on same machine

Installed:

* nginx
* docker
* docker-compose
* git
* ufw

Docker:

* Docker version 29.1.3
* docker-compose version 1.29.2

Users:

* root
* deploy

Main deployment user:

* deploy

Project structure:

/home/deploy/
├── apps/
├── data/
└── proxy/

Apps will be stored like:

/home/deploy/apps/admin-ui
/home/deploy/apps/client-ui
/home/deploy/apps/api

Architecture:

* nginx reverse proxy
* dockerized Next.js apps
* each app on separate internal port

Planned ports:

* 3001 admin-ui
* 3002 client-ui
* 4001 api

Domains/subdomains later routed through nginx.

Requirements:

* production-ready docker-compose
* optimized for Next.js
* restart always
* low RAM usage
* support environment variables
* support future SSL with certbot/nginx
* avoid root usage
* suitable for multiple apps on one VPS

Preferred deployment:

* git clone
* docker-compose up -d --build

Need:

* Dockerfile
* docker-compose.yml
* nginx config if needed
* recommended .dockerignore
* production optimization suggestions
