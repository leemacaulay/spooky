# spooky

spooky is a hauntingly convoluted way to:

- host a [Ghost](https://ghost.org/) blog in a Docker container;
- run its database using MySQL compatible [MariaDB](https://mariadb.org/) in another container;
- and serve it on a reverse proxy using [Traefik](https://traefik.io/) with free HTTPS provided by [Let's Encrypt](https://letsencrypt.org/).

This is _going to_ host my personal portfolio site, lmcly.fyi.

I put this together in a few hours on a bank holiday weekend by judiciously borrowing from other people's tutorials or blog posts.

I can't offer support, because I'm _just a journalist that knows how to google search error messages_, but I'll help where I can.

I'm still working on this and it's at an early stage so please **USE AT YOUR OWN RISK**.

## Configuration

spooky is mostly configured with .env files. These live in the `ghost` and `traefik` folders. You only really need to change `MYSQL_ROOT_PASSWORD`, `MYSQL_PASSWORD`, `GHOST_DOMAIN`. Even, `TRAEFIK_MONITOR_DOMAIN` is optional, but nice to have.

These are all written as key-value pairs: `GHOST_DOMAIN=example.com`

Here's what you need to know:

##### ghost/.env

env                 | defaults    | notes
--------------------|-------------|-------------------
MYSQL_ROOT_PASSWORD | changeme    |
MYSQL_DATABASE      | db          |
MYSQL_USER          | spooky_user |
MYSQL_PASSWORD      | changeme    |
GHOST_DOMAIN        | example.com |

##### traefik/.env

env                    | defaults             | notes
-----------------------|---------------------|-------------------
TRAEFIK_MONITOR_DOMAIN | monitor.example.com | optional

#### Setup

Before you start, you'll need:

- An Ubuntu 16.04 server with Docker and Docker Compose.
  - I recommend firing up a DigitalOcean [Docker Droplet](https://www.digitalocean.com/products/one-click-apps/docker/). $5 a month will get you 1GB memory, 25 GB storage
and 1 TB transfer bandwidth.
  - If you decide to go with DigitalOcean, you can amend and paste cloud-config.yaml by ticking the box [ ] User Data when setting up the droplet.
  - I've also got a [referral link that gets you $10 in credit](https://m.do.co/c/8107dde738c1) (Full transparency: I get $25 credit once someone who uses the link spends $25)
- A domain (example.com) and subdomain (monitor.example.com) pointing towards this server. I use [Cloudflare](cloudflare.com) for DNS.
---
1. Copy or git clone this repo to `/opt` or your home folders. If you choose `/opt` remember to prefix commands with `sudo` where needed.
2. In traefik/traefik.toml:
  - Add your email address in the [acme] section
  - In [entryPoints.webentry.auth.basic], replace `users = ["user:password"]` with a username and password you've generated using `htpasswd`.
    - The command to use is `htpasswd -nbm username password`
3. `touch traefik/acme.json` and fix its permissions by running `chmod 0600 traefik/acme.json`.
4. Enter your own `MYSQL_ROOT_PASSWORD`, `MYSQL_PASSWORD` and `GHOST_DOMAIN` in `ghost/.env`.
5. Enter your own `TRAEFIK_MONITOR_DOMAIN` in `traefik/.env`.
6. cd ../traefik and run `docker-compose up -d`
7. cd ../ghost and run `docker-compose up -d`
8. Head to your domain to set up your new blog - example.com/ghost

## Upgrading

Want to upgrade to the latest and greatest Ghost?

In ghost/build/Dockerfile, edit the first line to `FROM ghost:X.X` using the version number you need from the [official Docker Hub page for ghost](https://hub.docker.com/_/ghost/)

## Thanks & acknowledgements

A lot of the initial work for this is based on a tutorial post written by [Lorenz Vanthillo on Medium](https://medium.com/@lvthillo/deploy-ghost-publishing-platform-and-mysql-using-docker-compose-601b4b95afe7). That includes the idea to build the ghost container separately and add a wait-for-it script to wait for the MariaDB container to be ready.
