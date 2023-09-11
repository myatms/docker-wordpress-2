## Traefik Deployment (Example with Wordpress)

But before you get your Traefik container up and running, you need to create a configuration file and set up an encrypted password so you can access the monitoring dashboard.

You’ll use the htpasswd utility to create this encrypted password. First, install the utility, which is included in the apache2-utils package:

*	sudo apt-get install apache2-utils

Then generate the password with htpasswd. Substitute secure_password with the password you’d like to use for the Traefik admin user:

*	htpasswd -nb admin secure_password

The output from the program will look like this:

Output
admin:$apr1$ruca84Hq$mbjdMZBAG.KWn7vfN/SNK/

Running the Traefik Container
In this step you will create a Docker network for the proxy to share with containers. You will then access the Traefik dashboard. The Docker network is necessary so that you can use it with applications that are run using Docker Compose.

Create a new Docker network called web:

*	docker network create web

When the Traefik container starts, you will add it to this network. Then you can add additional containers to this network later for Traefik to proxy to.

Next, create an empty file that will hold your Let’s Encrypt information. You’ll share this into the container so Traefik can use it:

*	touch acme.json
  
Traefik will only be able to use this file if the root user inside of the container has unique read and write access to it. To do this, lock down the permissions on acme.json so that only the owner of the file has read and write permission.

*	chmod 600 acme.json
  
Once the file gets passed to Docker, the owner will automatically change to the root user inside the container.
