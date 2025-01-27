STANDARD DEPLOYMENT REPOSITORY :
--------------------------------

INFRASTRUCTURE :
----------------

The standard deployment procedure consist in mounting 3 software components using Docker Compose : Gateway, Linkyfront, Linkyback. Those components are linked together using a Docker network. 

    - Gateway : The gateway is a standard Nginx docker image, it's the only component exposed on the host and work as a generic entrypont. All the requests are reverse-proxied to linkyfront or linkyback regarding to the gateway.conf. This way none of your devices or browsers will ever contact the front-end or the back-end directly, you will be able to see all requests from the gateway logs and you can add additional configuration for advance request rules or filtering.

    - Linkyfront : Linkyfront is an image taken from the project official Docker hub repository https://hub.docker.com/r/flobtmkg/linkyreactapp. This image contains the already bundled Linky tracker React application served by an Nginx HTTP server on the default port 80 of the container. In the configuration proposed here, it can only be reached through the gateway.

    - Linkyback : Linkyback is an image taken from the project official Docker hub repository https://hub.docker.com/r/flobtmkg/linkydataserver. This image contains the already built Linky tracker data server Java Spring boot application and listen on the port 8080 of the container. In the the configuration proposed here, it can only be reached through the gateway.

Those components share a Docker brige network "internalAppsNetwork" to communicate so the gateway can dispatch the requests to Linkyfront or Linkyback.
The file named "gateway.conf" contains the Nginx configuration for the gateway.
A folder named "database" will be created after deployment, it is the location where the datas are persisted on the host disk.


COMMANDS :
----------

    - To deploy the application you have install Docker, download this repo and launch the command : "docker compose up -d".