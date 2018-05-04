# oidc-testing-playground
Some setup to be able to test an OIDC client

The problem: when you are trying to debug and test an OIDC client locally, it is difficult. There are two main players that play a huge role: the OIDC Server and the load balancer that sits in front of your service.

The second player, the load balancer, can be easy to forget if you are testing your application locally. However, when you are deploying your application to a cluster without sticky sessions, the load balancer plays a big role and its nice to be able to test this locally.

To create a test service, we use [keycloak](https://www.keycloak.org/). To create a test loadbalancer, we use [nginx](https://www.nginx.com/).

## Prerequisites

This setup uses [Docker](https://www.docker.com/) and bash.

## Setting up your playground

There are a few steps of setup for both the OIDC server and the loadbalancer.

1. Start you application locally on a few ports
3. Modify the `bin/nginx/nginx.conf`. The `server host.docker.internal:5677` lines should be modified so that you have one line for each instance of your service that is running and the port after `host.docker.internal` is the port that the instance is running on
4. Run `./bin/create`. This will create a keycloak docker image, create an OIDC client within that image (printing the client secret to the console), and set up and start an NGINX using the config in `bin/nginx/nginx.conf`

The options for the create script can be modified using environment variables:

* `CLIENT_ID` specifies the id of the client that will be generated (default: myclient)
* `NGINX_CONF` specifies the absolute path to the nginx config that will be used (by default, `bin/nginx.conf`)
* `REDIRECT_URL` for your application. Defaults to `http://localhost/*` which is the wildcard for the loadbalancer which is started with the nginx config

## Running the Playground

Once you've set up your playground, you can start it with `./bin/start` and stop it with `./bin/stop`

## Deleting the Playground

Delete everything with `./bin/remove`

## License

oidc-testing-playground is Open Source software released under the
[Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0.html).
