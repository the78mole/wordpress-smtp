# wordpress-smtp

Wordpress docker image with smtp

## Usage

First build your image:

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

Or adjust `wp-db-compose.yml` to your needs (mail credentials):

    docker compose -f wp-db-compose.yaml

As a first step, verify that smtp works (container name can vary):

    docker exec -it wordpress-smtp-wordpress-1 bash
    # Within container, execute
    echo -e "Subject: Testmail\n\nHello from container!" | sendmail test@mymail.com

If this sends you an email, also Wordpress should do.

## Development

The `wp-db-compose-build.yml' is for local building and testing the container.