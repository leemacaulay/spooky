# spooky

spooky is a hauntingly convoluted way to:

- host a [Ghost](add ghost link) blog in a Docker container;
- run its database using [Mariadb](add mariadb link) in another container;
- and serve it on a reverse proxy using [Traefik](add traefik link) with HTTPS provided by Letsencrypt

## Configuration

spooky is mostly configured with .env files. These live in the `ghost` and `traefik` folders. You only really need to change `MYSQL_ROOT_PASSWORD`, `MYSQL_PASSWORD`, `GHOST_DOMAIN`. Even, `TRAEFIK_MONITOR_DOMAIN` is optional, but nice to have.

Here's what you need to know:

##### ghost/.env

env                 | default     | optional/mandatory
--------------------|-------------|-------------------
MYSQL_ROOT_PASSWORD | changeme    |
MYSQL_DATABASE      | db          |
MYSQL_USER          | spooky_user |
MYSQL_PASSWORD      | changeme    |
GHOST_DOMAIN        | example.com |

##### traefik/.env

env                    | default             | optional/mandatory
-----------------------|---------------------|-------------------
TRAEFIK_MONITOR_DOMAIN | monitor.example.com |

#### Setup

1. Fire up a fresh server running docker with docker-compose installed
2. Copy or git clone this repo to /opt or your home folders
3. In traefik/traefik.toml:
  - Add your email address in the [acme] section
  - In [entryPoints.webentry.auth.basic], replace `users = ["user:password"]` with a username and password you've generated using `htpasswd`
    - The command to use is `htpasswd -nbm username password`
4. Fix the permissions on acme.json by running `chmod 0600 acme.json`
5. Follow the steps in Configuration above
6. cd ../traefik and run `docker-compose up -d`
7. cd ../ghost and run `docker-compose up -d`
8. Head to your domain to set up your new blog - example.com/ghost
