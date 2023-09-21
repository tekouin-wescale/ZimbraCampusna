
# Zimbra
Updated Version of <https://hub.docker.com/r/jorgedlcruz/zimbra>

In this Repository you will find how to install Zimbra on Docker

# Docker

## How to install Docker

Depend of your OS, you need to install Docker in different ways, take a look into the official Website - <https://docs.docker.com/engine/installation/>

## Downloading the image

The first step is to pull this image into your docker environment, for that just run the next:

```bash
docker pull campusnaa/zimbra
```

## Creating Zimbra Containers

Now that we have an image called campusnaa/zimbra, we can do a docker run with some special parameters, like this:

```bash
docker run -p 25:25 -p 80:80 -p 465:465 -p 587:587 -p 110:110 -p 143:143 -p 993:993 -p 995:995 -p 443:443 -p 8080:8080 -p 8443:8443 -p 7071:7071 -p 9071:9071 -h zimbra-docker.zimbra.io --dns 127.0.0.1 --dns 8.8.8.8 -i -t -e PASSWORD=Zimbra2017 campusnaa/zimbra:latest
```

As you can see we tell the container the ports we want to expose, and on which port, we also specify the container hostname, the password for the Zimbra Administrator Account, and the image to use.

That's it! You can visit now the IP of your Docker Machine using HTTPS, or try the Admin Console with HTTPS and port 7071.

## Contribute to the Project

If you like to contribute to the project, you are free to do so, just fork this repo and submit your changes.

# Manual process - not really recommended

<details>
  <summary>Manual process</summary>

## Creating the Zimbra Image

The content of the Dockerfile and the start.sh is based on the next Script - ZimbraEasyInstall. The Dockerfile creates a Ubuntu Server 16.04 image and install on it all the OS dependencies which Zimbra needs, then when the container is launched, automatically starts with the start.sh script which creates an auto-config file which is injected during the zimbra Installation.

### Using git

Download from github, you will need git installed on your OS

```bash
git clone https://github.com/tekouin-wescale/ZimbraCampusna.git
```

### Using wget

For those who want to use wget, follow the next instructions to download the Zimbra-docker package. You might need wget and unzip installed on your OS

```bash
wget https://github.com/tekouin-wescale/ZimbraCampusna/archive/main.zip
unzip main.zip
```

### Build the image using the Dockerfile

The `Makefile` in the docker/ directory provides you with a convenient way to build your docker image. You will need make on your OS. Just run

```bash
cd Campusnazimbra/docker
sudo make
```

The default image name is Campusnazimbra.

### Deploy the Docker container

Now, to deploy the container based on the previous image. As well as publish the Zimbra Collaboration ports, the hostname and the proper DNS, as you want to use bind as a local DNS nameserver within the container, also we will send the password that we want to our Zimbra Server like admin password, mailbox, LDAP, etc.: Syntax:

```bash
docker run -p PORTS -h HOSTNAME.DOMAIN --dns DNS_SERVER -i -t -e PASSWORD=YOUR_PASSWORD NAME_OF_DOCKER_IMAGE
```

Example:

```bash
docker run -p 25:25 -p 80:80 -p 465:465 -p 587:587 -p 110:110 -p 143:143 -p 993:993 -p 995:995 -p 443:443 -p 8080:8080 -p 8443:8443 -p 7071:7071 -p 9071:9071 -h zimbra-docker.zimbra.io --dns 127.0.0.1 --dns 8.8.8.8 -i -t -e PASSWORD=Zimbra2017 campusnaa/zimbra:latest
```

This will create the container in few seconds, and run automatically the start.sh:

* Install a DNS Server based in dnsmasq
* Configure all the DNS Server to resolve automatically internal the MX and the hostname that we define while launch the container.
* Install a fresh Zimbra Collaboration 10.0.0 within Zimbra Chat and Drive!
* Create 2 files to automate the Zimbra Collaboration installation, the keystrokes and the config.defaults.
* Launch the installation of Zimbra based only in the .install.sh -s
* Inject the config.defaults file with all the parameters that is auto configured with the Hostname, domain, IP, and password that you define before.

The script takes a few minutes, dependent on the your Internet Speed, and resources.

</details>

## Known issues

After the Zimbra automated installation, if you close or quit the bash console from the container, the docker container might exit and keep in stopped state, you just need to run the next commands to start your Zimbra Container:

```bash
docker ps -a 
docker start YOUR_CONTAINER_ID
docker exec -it YOUR_CONTAINER_ID bash
su - zimbra
zmcontrol restart
```
