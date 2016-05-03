# Drupal Social #
Check [drupal.org](https://www.drupal.org/project/social) for more information.
For day to day technical documentation use the [Github Wiki](https://github.com/goalgorilla/drupal_social/wiki).

Hello DrupalJam

Useful links for developers:
- [Roadmap](https://drupalsocial.storiesonboard.com/m/drupal-social-mvp)
- [Lo-fi prototype](http://prototype.goalgorilla.com/drupalsocial/current/)
- [Hifi styleguide and prototype](http://goalgorilla.github.io/drupal_social/)

# Commit messages guidelines #
[Jira](https://goalgorilla.cloudshards.net/secure/Dashboard.jspa) is used for the issue queue. In order to link the commits to profiles at drupal.org when we release the distribution use the following [template](https://www.drupal.org/node/52287):

```
[DS-#Issue] by [comma-separated usernames]: [Summary of the change]
```
For example:
```
DS-123 by nielsvandermolen, ronaldtebrake: Login with email and custom validation constraint on create account username field.
```

# Installation #

## Docker toolbox ##
Download and install the [toolbox](https://www.docker.com/docker-toolbox).

Note that the docker projects have to be somewhere in your /Users/ directory in order to work (limitation for Mac and Windows). Note that /Users/<name>/Sites/Docker is fine.


## Steps ##

1. Start a docker machine (docker quickstart icon).

2. Clone this repository to the directory of your choice (e.g. ~/Sites/social).

3. Go inside the folder in which you cloned this repository (where the docker-compose.yml file is).

4. Build and start the docker containers.
    ```
    docker-compose up -d
    ```

    This will build multiple containers (see the Dockerfile in docker_build/drupal8) and all the dependencies.

5. Add social.dev and mailcatcher.social.dev to your /etc/hosts file based on the ip of the docker machine.

    If necessary you can find the IP with this command on your host machine:
    ```
    docker-machine ls
    ```

6. Run the install script on your host machine.
    ```
    sh docker_build/drupal8/install_script.sh
    ```

7. Add the proxy container.
    ```
    docker run -d -p 80:80 --name=proxy -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
    ```

## Usage ##

**If you want to see which containers are running:**
```
docker ps
```

**SSH into the container:**
```
docker exec -it social_web_1 bash
```
Here you can use _drush_ and _drupal list_.

**If you want to re-install, execute these commands in the social_web_1 container:**
```
drush sql-drop -y;
rm -f sites/default/settings.php
rm -f sites/default/settings.local.php
rm -rf sites/default/files
```

**If you want to start the proxy:**
```
docker start proxy
```

**Now run the install script on your host machine again.**
```
docker exec -it social_web_1 bash /root/dev-scripts/install/install_script.sh
```

**To view emails go to:**
```
mailcatcher.social.dev
```

[![Build Status](https://travis-ci.org/goalgorilla/drupal_social.svg?branch=master)](https://travis-ci.org/goalgorilla/drupal_social)
