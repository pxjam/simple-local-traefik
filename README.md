# Simple Local Traefik

[Traefik](https://docs.traefik.io/) is an open-source edge router that intelligently proxies incoming network traffic to other services. You could run a hosting company with it, or run your local apps with `.test` domains and full HTTPS.

This project focuses on the simplest steps needed to setup a local Traefik environment so you can start testing with real domain names. Whether for vanity or otherwise.

## Instructions

1. Install [Docker](https://docs.docker.com/docker-for-mac/install/) and [Homebrew](https://brew.sh/).

2. Add `traefik.test` and any desired `.test` domains to `/etc/hosts`.

   ```bash
   127.0.0.1  traefik.test
   127.0.0.1  my-site.test
   ```

3. Create an external Docker network called `web` that will be used to connect traefik to other services (e.g. a Docker container running an nginx image).

   ```bash
   docker network create web
   ```

4. Install [`mkcert`](https://github.com/FiloSottile/mkcert) (and [`nss`](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS) if you're using Firefox).

   ```bash
   brew install mkcert
   brew install nss
   ```

5. Setup a local root certificate authority (CA) using `mkcert`.

   ```bash
   mkcert -install
   ```

6. Clone this repository

   ```
   git clone https://github.com/pxjam/simple-local-traefik.git
   cd simple-local-traefik
   ```

7. Create a TLS certificate/key files for `traefik.test` and `my-site.test` and and store in `certs` folder within the repository. Repeat for each of your domains.

   ```bash
   mkcert -cert-file certs/traefik.crt -key-file certs/traefik-key.pem "traefik.test"
   mkcert -cert-file certs/mysite.crt -key-file certs/mysite-key.pem "my-site.test"
   ```

8. Start docker-compose project

   ```sh
   docker compose up -d
   ```

9. Visit https://traefik.test/dashboard/ and https://my-site.test in your browser.
