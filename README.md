# GoReplay Docker Compose example

An example of using goreplay inside docker-compose

## Introduction

[GoReplay](https://goreplay.org/) is a simple, self contained binary that allows
you to replay traffic from one target against a different one. One great use case
for this is replaying traffic from your production environment to your staging
one to ensure it can handle the load in a timely and correct way.

This repository contains a docker-compose file that will build a `goreplay`
container and deploy two nginx instances. Any traffic sent to the
`frontendnginx` listening on port `8080` will be seen by `goreplay` and
also be sent to the `backendnginx` on port `8181`.

## Running the containers

Start by running the containers using Docker. The logs will be printed to stdout.

    docker-compose up

    Recreating gorexperiment_goreplay_1    ... done
    Starting gorexperiment_backendnginx_1  ... done
    Starting gorexperiment_frontendnginx_1 ... done


On the first run `docker-compose` will build the `goreplay` container using the
binary from the GitHub release page. This will only happen once and all future
startups will be much faster. We can now make a request to the `frontendnginx`:

    curl -I 127.0.0.1:8080?composed=true

    HTTP/1.1 200 OK
    Server: nginx/1.14.1

If everything is working this request will be visible in the `docker-compose`
terminal:

    frontendnginx_1  | 172.20.0.1 - - [27/Nov/2018:08:49:29 +0000] "GET /?composed=true HTTP/1.1" 200 612 "-" "curl/7.59.0" "-"
    backendnginx_1   | 172.20.0.1 - - [27/Nov/2018:08:49:31 +0000] "GET /?composed=true HTTP/1.1" 200 612 "-" "curl/7.59.0" "-"

## Where to next?

I originally created this example to both test a newer version of
`goreplay` and to learn how to sniff traffic inside a `docker-compose`
based deployment. While this example is very standalone the
configuration and network behaviours can be reused when building your
own testing environments.

### Author

[Dean Wilson](https://www.unixdaemon.net)
