Please see the official [Caddy Docker Image](https://hub.docker.com/_/caddy) for deployment instructions.

Use:

1. You should add CLOUDFLARE_EMAIL and CLOUDFLARE_API_TOKEN as environment variables to your `docker run` command. Example:

    ```
    docker run -it --name caddy \
      -p 80:80 \
      -p 443:443 \
      -v caddy_data:/data \
      -v caddy_config:/config \
      -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
      -e CLOUDFLARE_EMAIL=me@example.com \
      -e CLOUDFLARE_API_TOKEN=12345 \
      -e ACME_AGREE=true \
      ghcr.io/retnil/caddy-cloudflare:latest
    ```

    You can obtain your [Cloudflare API token](https://support.cloudflare.com/hc/en-us/articles/200167836-Managing-API-Tokens-and-Keys) via the Cloudflare Portal. To create a API token with minimal scope, the following steps are needed:

    1. Log into your dashboard, go to account settings, create API token
    2. grant the following permissions:

        - Zone / Zone / Read
        - Zone / DNS / Edit

2. You should add the following to your Caddyfile as the [tls directive](https://caddyserver.com/docs/caddyfile/directives/tls#tls).

    ```
    tls {$CLOUDFLARE_EMAIL} {
      dns cloudflare {$CLOUDFLARE_API_TOKEN}
    }
    ```
