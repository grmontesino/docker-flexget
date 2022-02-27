# grmontesino/flexget container image

![Docker CI](https://github.com/grmontesino/docker-flexget/actions/workflows/build.yml/badge.svg)

Container image and sample docker compose file for flexget by Gustavo R. Montesino.

"Flexget is a multipurpose automation tool for all of your media" - for details please see the official website: https://flexget.com/

## Disclaimer
This container has been made for myself and is provided as-is, without warranty of any kind, in the hope it can be useful.

While I don't think there is anything particularly novel or copyright-worthy in this repo, feel free to re-use any of this code however you see fit. Please consider linking to the original and/or letting me know if you do use it.

## Usage

### Getting started

Flexget configuration is very flexible, and by consequence can have lots of variations and levels of complexity depending on what is intended. That being the case, I've decided not to try to expose the configuration as environment variables for this image.

Please check flexget's documentation and create your config.yml file. For use with the provided docker-compose yaml, the configuration file should be saved on the config dir.

Flexget will be run with uid 110 by default. to avoid file permission erros, set the config dir and it's contents to this user or change the user to another one with sufficient permissions (see "Container runtime parameters" below).

To validate the sintaxe flexget configuration, run `docker-compose run --rm flexget check`.

To start flexget as daemon, run `docker-compose up -d`.

Logs can be checked by running `docker-compose logs` or by checking the `flexget.log` file directly on the `config` folder.

### Container runtime parameters

The following parameters from the `docker-compose.yaml` file can be customized without the need to edit the file directly, simplifying future updates. To do so, create an .env file on the same directory of the compose file and include the desired values, as "NAME=VALUE" pairs.

| Variable | Description |
|:---------|:------------|
| FG_TAG | Image tag |
| FG_UID | UID of the user which will run the daemon |
| FG_TZ | Timezone |

Sample .env file:

```
FG_TAG=3.2.4
FG_UID=1000
FG_TZ=America/Sao_Paulo
```

### Flexget plugins

This image has been configured with the dependencies to the following flexget plugins. Other plugins might work, or not.

* telegram
* transmission

### Volumes

This image uses one volume, mounted at "/flexget". Config file will be read from this volume, and flexget files (database, logs, etc) get saved on the same place. On the provided docker compose file this is mapped to the "config" directory of this repository.

### Network

The provided docker compose file will create/connect to a network named `torrent`. This works well with my [transmission image](https://github.com/grmontesino/docker-transmission)

### Updating

Before updating:

* check this repository changelog for relevant changes which might require some configuration change;
* Likewise, check the [flexget upgrade actions](https://flexget.com/UpgradeActions), especially if the first or second digit of the version/tag number got changed.

If no changes were made to the compose file, just `git pull` the updated files and `docker-compose up -d` to start/update the container.
