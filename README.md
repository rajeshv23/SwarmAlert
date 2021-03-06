[![Docker](https://raw.githubusercontent.com/USDevOps/mywechat-slack-group/master/images/docker.png)](https://cloud.docker.com/u/gpulidodt/repository/docker/gpulidodt/swarm-alert)
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu/#/en_US)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://github.com/gpulido/SwarmAlert/blob/master/LICENSE)
[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)

# Introduction
The SwarmAlert app monitors the availability of services running in a Docker Swarm environment. 

It offers an optional WHITELIST of services to monitor. If a WHITELIST is not defined, ALL services in the Swarm are monitored by default. 

An optional BLACKLIST is also configurable, and takes precedence over the whitelist and defaults. This is to allow you to use the BLACKLIST to avoid receiving alerts while doing planned maintenance. The app checks every CHECK_INTERVAL seconds. Upon checking, if a specified service has no running task, the app generates a Notification that is send to the services configured on the config.yml file.
More information about the config.yml format is available on the apprise repository:  [apprise config yaml file reference](https://github.com/caronc/apprise/wiki/config_yaml)

The Msg_Prefix is used to customize the message send to the notification providers.  

Note: This project is based on [monitor-docker-slack](https://github.com/DennyZhang/monitor-docker-slack)

# General Idea
1. Run one or more services in Docker Swarm mode.
2. Start this service to monitor the availability of other services.
3. Monitor services each CHECK_INTERVAL seconds for services with no running containers.
4. Send a pushover notifications containing the unavailable services.

# Usage: Docker-compose
```
version: '3'
services:
  swarm-alert:
    image: gpulidodt/swarm-alert:latest
    volumes:
     - /var/run/docker.sock:/var/run/docker.sock
     - /path_to_config/config.yml:/src/config.yml
    environment:
     - PUSHOVER_USER_KEY: user_key_from_pushover
     - PUSHOVER_API_TOKEN: your_app_token_from_pushover
     - MSG_PREFIX: '$MSG_PREFIX'
     - WHITE_LIST: ''traefik_traefik','plex_plex','test_service''
     - BLACK_LIST: ''test_service''
     - LOGGING_LEVEL: INFO | DEBUG
```

# Further customization
- Customize the message prefix for the Pushover Notification via the MSG_PREFIG variable:
```
 - MSG_PREFIX="Swarm services"
```
 - If defined, the services that are monitored are those on the WHITE_LISTE_LIST variable:
```
 - WHITE_LIST="nodeexporter,ngin.*"
```
 - If defined, the services defined on the BLACK_LIST are excluded from monitoring:
```
 - BLACK_LIST="nodeexporter,ngin.*"
```
- Logging capabilities are included, it is set as INFO level by default, can be increased to DEBUG using the LOGGIN_LEVEL env variable.

Code is licensed under MIT license


