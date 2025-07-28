# wordpress-smtp

Wordpress docker image with smtp

NOTE: It seems, that wordpress still has problems with this mail config. Use e.g. WP Mail SMTP within Wordpress to send Emails.

## Usage

### Using pre-built image

The easiest way is to use the pre-built image from GitHub Container Registry:

    docker run -it --rm \
      -p 8080:80 \
      -e SMTP_HOST=smtp.mailbox.org \
      -e SMTP_PORT=587 \
      -e SMTP_USER=deinuser@mailbox.org \
      -e SMTP_PASS=deingeheim \
      -e SMTP_FROM=wordpress@e-glaser.de \
      -e WORDPRESS_DB_HOST=localhost:3306 \
      -e WORDPRESS_DB_USER=wpuser \
      -e WORDPRESS_DB_PASSWORD=geheim \
      -e WORDPRESS_DB_NAME=wp \
      ghcr.io/the78mole/wordpress-smtp:latest

### Building locally

If you want to build the image yourself:

    docker build -t wordpress-smtp:test .
    podman build -t wordpress-smtp:test .

When this is finished, you can run the container with the correct env set (you need a running mariadb):

    docker run -it --rm \
      -p 8080:80 \
      -e SMTP_HOST=smtp.mailbox.org \
      -e SMTP_PORT=587 \
      -e SMTP_USER=deinuser@mailbox.org \
      -e SMTP_PASS=deingeheim \
      -e SMTP_FROM=wordpress@e-glaser.de \
      -e WORDPRESS_DB_HOST=localhost:3306 \
      -e WORDPRESS_DB_USER=wpuser \
      -e WORDPRESS_DB_PASSWORD=geheim \
      -e WORDPRESS_DB_NAME=wp \
      wordpress-smtp:test

Or use the pre-built image with docker-compose. Adjust `wp-db-compose.yml` to your needs (mail credentials):

    docker compose -f wp-db-compose.yml up -d

Alternatively, if you want to build locally, use:

    docker compose -f wp-db-compose-build.yml up -d --build

As a first step, verify that smtp works (container name can vary):

    docker exec -it wordpress-smtp-wordpress-1 bash
    # Within container, execute
    echo -e "Subject: Testmail\n\nHello from container!" | sendmail test@mymail.com

If this sends you an email, also Wordpress should do.

## Development

The `wp-db-compose-build.yml` is for local building and testing the container.

## Automated Releases

This project uses GitHub Actions for automated building and releasing:
- **Semantic Versioning**: Commits following conventional commit format trigger automatic version bumps
- **Docker Images**: Automatically built and pushed to GitHub Container Registry
- **GitHub Releases**: Created automatically with each new version

Commit message patterns:
- `feat:` - Minor version bump (new feature)
- `fix:` - Patch version bump (bug fix)  
- `feat!:`, `fix!:`, `refactor!:` - Major version bump (breaking change)