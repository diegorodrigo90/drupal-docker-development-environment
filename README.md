# Drupal development Docker environment

This is an easy Drupal development environment created based on [Drupal Oficial Documentation](https://www.drupal.org/docs/develop/local-server-setup/docker-development-environments/docker-with-solr-cloud-integration/docker-configuration).

## Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [PHP](https://www.php.net/)
- [Composer](https://getcomposer.org/)

## Before run

### Create Certs

```bash
mkdir certs
```

```bash
cd certs
```

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.cert
```

The only important question while running the openssl command is Common Name (e.g. server **FQDN** or **YOUR** name):. It should match the domain name of your server [in our case **drupal.mysite.localhost**]

Change the certs permissions:

```bash
chmod 644 server.cert
```

```bash
chmod 600 server.key
```

back to root directory:

```bash
cd ..
```

### Configure Solr

```bash
mkdir solr-cloud
cd solr-cloud
wget https://raw.githubusercontent.com/drud/ddev-contrib/master/docker-compose-services/solr/solr/security.json
cd ..
```

```bash
mkdir solr-cloud
```

```bash
cd solr-cloud
```

```bash
wget https://raw.githubusercontent.com/drud/ddev-contrib/master/docker-compose-services/solr/solr/security.json
```

back to root directory:

```bash
cd ..
```

### Configure your hosts file

Add the following hosts to yout hosts file:

```
127.0.0.1 drupal.mysite.localhost
127.0.0.1 solr.mysite.localhost
127.0.0.1 portainer.mysite.localhost
```

Host files can be found in:
Windows: `C:\Windows\System32\Drivers\etc\hosts`
Linux: `/etc/hosts`

### Install composer dependencies

Install the composer dependencies:

```bash
composer install
```

Answer `y` for any question.

### Running containers

Now that we have all things set up all we have to do is run the containers:

For Drupal, Solr and Zookeeper:

```bash
docker-compose -f "docker-compose.yml" up -d
```

Database, Mailhog, Portainer and Traefik:

```bash
docker-compose -f "docker-compose.override.yml" up -d
```

### Create translations folder

**If you will use an language different from english** create translations folder in `web/sites/default/files/translations`

```bash
mkdir web/sites/default/files/translations
```

```bash
chmod 777 web/sites/default/files/translations
```

### Installing Drupal

Go to https://drupal.mysite.localhost/ and follow the steps to install Drupal, database default options are:

**MYSQL Server:** mariadb.mysite.localhost
**MYSQL port:** 3306
**MYSQL root password**: MyGreatPassword
**MYSQL database:** drupal
**MYSQL user:** drupal
**MYSQL password:** drupal


