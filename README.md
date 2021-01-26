# Interoperability Test Platform

To run the ITP:

1. Adjust .env, app.env and mysql.env for your environment (see below for help with generating `APP_KEY`)
2. Adjust nginx.conf to replace "example.com" with your own URL
3. Adjust docker-compose.yml:

   1. Select a specific
      [version](https://hub.docker.com/repository/docker/gsmainclusivetechlab/interop-test-platform/tags?page=1&ordering=last_updated&name=v)
      to deploy instead of using `v1.2.0` under `x-common-php.image`
   2. Change `services.app.ports` if you do not wish to serve on ports 80/443
   3. Uncomment `services.mailhog` if you wish to run the test mail server (and adjust ports if desired)
   4. Uncomment `services.phpmyadmin` if you wish to run PHPMyAdmin (and adjust ports if desired)

4. Launch the platform with:

```bash
docker-compose up -d
```

5. Wait for the containers to start up, then setup the database:

```bash
docker-compose exec app sh
php artisan migrate:refresh --seed
```

Note that if you want to update an existing database, you should use `php artisan migrate` instead.

6. On first launch, ITP will launch using self-signed certificates to secure
   the HTTPS connection. To generate new certificates for your domain using
   letsencrypt, ensure that .env is correctly updated, then run the following:

```bash
docker-compose exec certbot sh
certbot certonly --webroot -w /var/www/certbot -d ${PROJECT_DOMAIN} -m you@your-domain.com
```

After running this, edit `nginx.conf` to uncomment the entries labelled `ssl_certificate` and `ssl_certificate_key` and remove the self-signed localhost keys instead.

7. Seeding configuration [TODO]
8. Configure locales [TODO]
9. Configure theming

## Troubleshooting

### Generating APP_KEY

```
docker-compose exec app sh
vi .env # add `APP_ENV=` to the bottom of the .env file
php artisan key:generate
cat .env # copy APP_ENV outside of here into app.env
```
