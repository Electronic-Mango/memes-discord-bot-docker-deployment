# Memes Discord bot Docker deployment

This repository provides an example of how you can combine my two other projects - [Random media Discord bot](https://github.com/Electronic-Mango/random-media-discord-bot) and [Reddit API API](https://github.com/Electronic-Mango/reddit-api-api) - into a **Memes Discord bot** via [Docker Compose](https://docs.docker.com/compose/).



## Configuration

### Reddit API API

[**Reddit API API**](https://github.com/Electronic-Mango/reddit-api-api) configuration is stored in `./reddit-api-api/config/custom_settings.yml` file.
This file isn't loaded into the Docker image, it's stored in Docker container mounted volume as `/config/custom_settings.yml`.

`docker-compose.yml` configures `CUSTOM_SETTINGS_PATH` environment variable to point to correct settings YAML.

In order for the API to work you have to [create a Reddit app](https://old.reddit.com/prefs/apps/) and supply its ID and secret in `./reddit-api-api/config/custom_settings.yml` in this project:

```yaml
reddit:
  client:
    # Your Reddit app ID
    id: <reddit-app-id>
    # Your Reddit app secret
    secret: <reddit-app-secret>
```

You can use `./reddit-api-api/config/custom_settings.yml` for additional configuration of Reddit API API, but for this bot only those two parameters are necessary.

More detailed description of configuration is available in [**Reddit API API**](https://github.com/Electronic-Mango/reddit-api-api) project and its default settings YAML.
