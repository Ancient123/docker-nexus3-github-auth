![docker stars](https://img.shields.io/docker/stars/lokkju/nexus3-github-auth.svg)
![docker pulls](https://img.shields.io/docker/pulls/lokkju/nexus3-github-auth.svg)
![docker automated](https://img.shields.io/docker/automated/lokkju/nexus3-github-auth.svg)
![docker build status](https://img.shields.io/docker/build/lokkju/nexus3-github-auth.svg)

## Credits
This docker image is based on [sonatype/docker-nexus3](https://github.com/sonatype/docker-nexus3) with the [nexus3-github-oauth-plugin](https://github.com/larscheid-schmitzhermes/nexus3-github-oauth-plugin/) enabled.

## Run the docker image
```
$ docker volume create --name nexus-data
$ docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data lokkju/nexus3-github-auth
```

## Configure Github Authentication

#### 1. Activate the Realm
Log in to your nexus and go to _Administration > Security > Realms_. Move the Github Realm to the right. The realm order in the form determines the order of the realms in your authentication flow. We recommend putting Github _after_ the built-in realms:
![setup](https://github.com/larscheid-schmitzhermes/nexus3-github-oauth-plugin/raw/master/setup.png)

#### 2. Group / Roles Mapping
When logged in through Github, all organizations and teams the user is a member of will be mapped into roles like so:

_organization name/team name_ e.g. `dummy-org/developers`

You need to manually create these roles in _Administration > Security > Roles > (+) Create Role > Nexus Role_ in order to assign them the desired priviliges. Note that anybody is allowed to login (authenticate) with a valid Github Token from your Github instance, but he/she won't have any priviledges assigned with their teams (authorization).

![role-mapping](https://github.com/larscheid-schmitzhermes/nexus3-github-oauth-plugin/raw/master/role-mapping.png)

## Usage

The following steps need to be done by every developer who wants to login to your nexus with Github.
#### 1. Generate OAuth Token
 
In your github account under _Settings > Personal access tokens_ generate a new OAuth token. The only scope you need is **read:org** 

#### 2. Login to nexus

When logging in to nexus, use your github user name as the username and the oauth token you just generated as the password.
This also works through maven, gradle etc.
