# Interoperability Test Platform

To run the ITP:

1. Adjust .env, app.env and mysql.env for your environment
2. Adjust nginx.conf to replace "example.com" with your own URL (in 4 locations)
3. Adjust docker-compose.yml:

   1. Change `services.app.ports` if you do not wish to serve on ports 80/443
   2. Uncomment `services.mailhog` if you wish to run the test mail server (and adjust ports if desired)
   3. Uncomment `services.phpmyadmin` if you wish to run PHPMyAdmin (and adjust ports if desired)

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
certbot certonly --webroot -w /var/www/certbot -d ${PROJECT_DOMAIN} --register-unsafely-without-email --agree-tos
```

7. Seeding configuration [TODO]
8. Configure locales [TODO]
