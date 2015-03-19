# docker-nginx-ember-proxy
Docker for hosting Ember apps in Nginx over HTTPS with a backend API. All requests starting with `/api/` are passed to the backend.

## Running your Ember app
    docker run --name my-ember-app \
        --link my-backend-container:backend \
        -v /path/to/ember/dist:/usr/share/nginx/html:ro \
        -v /path/to/certificates:/certs \
        -d erikap/nginx-ember-proxy

The certificates folder (`/path/to/certificates`) must contain the following files:
- app.csr
- app.crt
- app.key
- dhparam.pem

## Creating a self-signed certificate
Execute the following commands to create a self-signed certificate:

    openssl genrsa -out app.key 2048
    openssl req -new -key app.key -out app.csr
    openssl x509 -req -days 365 -in app.csr -signkey app.key -out app.crt
    openssl dhparam -out dhparam.pem 2048
