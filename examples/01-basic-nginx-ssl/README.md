## Simple OroCommerce application with nginx reverse proxy

See nginx config [here](nginx-site.conf)

```yaml
version: '2'

services:
    postgres:
        hostname: postgres
        container_name: postgres_orocommerce
        image: postgres:9.6
        volumes:
            - .docker/db:/var/lib/postgresql/data
        environment:
            POSTGRES_DB: orocommerce
            POSTGRES_PASSWORD: 123456
        expose:
            - "5432"

    orocommerce:
        image: okvpn/orocommerce:3.1
        container_name: orocommerce
        hostname: orocommerce
        ports:
            - 127.0.0.1:8085:80
        volumes:
            - .docker/attachment:/var/www/orocommerce/var/attachment
            - .docker/media:/var/www/orocommerce/public/media
            - .docker/uploads:/var/www/orocommerce/public/uploads
        environment:
            # installation options
            APPLICATION_URL: https://orocommerce.companyname.org
            ADMIN_USER: admin
            ADMIN_PASSWORD: admin
            ADMIN_EMAIL: admin@example.com
            ADMIN_FIRST_NAME: Alieś
            ADMIN_LAST_NAME: Zagorski
            ORGANIZATION_NAME: Oro
            TRUSTED_PROXIES: 172.16.0.0/12 # See index.php
#            ORO_CONSUMER_COUNT: 1 # By default 1

#            Parameters.yml options
            ORO_DATABASE_DRIVER: pdo_pgsql
            ORO_DATABASE_HOST: postgres
            ORO_DATABASE_PORT: 5432
            ORO_DATABASE_USER: postgres
            ORO_DATABASE_NAME: orocommerce
            ORO_DATABASE_PASSWORD: 123456
            # mailer options 
#            ORO_MAILER_TRANSPORT: smtp
#            ORO_MAILER_HOST: smtp.mailgun.org
#            ORO_MAILER_PORT: 587
#            ORO_MAILER_USER: user@example.com
#            ORO_MAILER_PASSWORD: 123456
            
#            ORO_WEBSOCKET_FRONTEND_HOST: *
#            ORO_WEBSOCKET_FRONTEND_PORT: 443
#            ORO_WEBSOCKET_FRONTEND_PATH: ws
#            ORO_WEB_BACKEND_PREFIX: /admin
#            ORO_LOCALE: en

            # Don't change SECRET after install
            # Must be permanent, because used to encrypt data in database
            ORO_SECRET: 1HYCxPwrHDUPOYupNKsqvoOM5Q
```
