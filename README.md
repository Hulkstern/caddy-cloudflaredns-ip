[![Latest Release][version-image]][version-url]
[![caddy on DockerHub][dockerhub-image]][dockerhub-url]
[![Docker Build][gh-actions-image]][gh-actions-url]

# caddy-cloudflaredns-ip

This repository is a fork of [SlothCroissant/caddy-cloudflaredns](https://github.com/SlothCroissant/caddy-cloudflaredns/) that adds the community created ip source plugin for Cloudflare IP addresses in addition to the official Cloudflare DNS plugin already added by SlothCroissant.

Please see the official [Caddy Docker Image](https://hub.docker.com/_/caddy) for deployment instructions.

Builds are available at the following Docker repositories:

* Docker Hub: [docker.io/hulkstern/caddy-cloudflaredns-ip](https://hub.docker.com/r/hulkstern/caddy-cloudflaredns-ip)
* GitHub Container Registry: [ghcr.io/hulkstern/caddy-cloudflaredns-ip](https://ghcr.io/hulkstern/caddy-cloudflaredns-ip)
* Quay Container Registry: [quay.io/hulkstern/caddy-cloudflaredns-ip](https://quay.io/repository/hulkstern/caddy-cloudflaredns-ip)

Few things to note: 

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
        slothcroissant/caddy-cloudflaredns:latest
      ```
      
      You can obtain your [Cloudflare API token](https://support.cloudflare.com/hc/en-us/articles/200167836-Managing-API-Tokens-and-Keys) via the Cloudflare Portal. To create a API token with minimal scope, the following steps are needed:

   1. Log into your dashboard, go to account settings, create API token
   2. grant the following permissions:

      * Zone / Zone / Read
      * Zone / DNS / Edit
      
2. You should add the following to your Caddyfile as the [tls directive](https://caddyserver.com/docs/caddyfile/directives/tls#tls). 

   ```
   tls {$CLOUDFLARE_EMAIL} { 
     dns cloudflare {$CLOUDFLARE_API_TOKEN}
   }
   ```

3. This image now supports tagging! [See available tags here](https://hub.docker.com/r/slothcroissant/caddy-cloudflaredns/tags). To select a specific version of `caddy`, set your [Docker image tag](https://docs.docker.com/engine/reference/run/#imagetag) to the caddy version you'd like to use. 

   Example: `slothcroissant/caddy-cloudflaredns:2.4.3`

[version-image]: https://img.shields.io/github/v/release/SlothCroissant/caddy-cloudflaredns?style=for-the-badge
[version-url]: https://github.com/SlothCroissant/caddy-cloudflaredns/releases

[gh-actions-image]: https://img.shields.io/github/actions/workflow/status/SlothCroissant/caddy-cloudflaredns/main.yml?style=for-the-badge
[gh-actions-url]: https://github.com/SlothCroissant/caddy-cloudflaredns/actions

[dockerhub-image]: https://img.shields.io/docker/pulls/slothcroissant/caddy-cloudflaredns?label=DockerHub%20Pulls&style=for-the-badge
[dockerhub-url]: https://hub.docker.com/r/slothcroissant/caddy-cloudflaredns
