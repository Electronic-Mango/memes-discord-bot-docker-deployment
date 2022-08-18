# Memes Discord bot Docker deployment

This repository provides an example of how you can combine my two other projects - [Random media Discord bot](https://github.com/Electronic-Mango/random-media-discord-bot) and [Reddit API API](https://github.com/Electronic-Mango/reddit-api-api) - into a **Memes Discord bot** via [Docker Compose](https://docs.docker.com/compose/).

Both Reddit API API and Random media Discord bot are stored in this repo as git submodules.



## Updates to source code of submodules

After cloning this repository you have to update submodules to their correct versions.
Otherwise you won't be able to build their Docker images.
You can update the submodules via:

```bash
git submodule update --init
```

After future update to submodules you have to update them after rebasing this repository via:

```bash
git submodule update
```

When Docker Compose is run with `--build` flag a new Docker images will be built, using Dockerfile in submodules, so it is important that they are up to date.

This and submodules repositories on GitHub use workflows to ensure, that source code here is always up to date.



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

Other parameters will use their default values stores in `settings.yml` file in Reddit API API itself.

You can use `./reddit-api-api/config/custom_settings.yml` for additional configuration of Reddit API API, but for this bot only those two parameters are necessary.

More detailed description of configuration is available in [**Reddit API API**](https://github.com/Electronic-Mango/reddit-api-api) project and its default settings YAML.


### Random media Discord bot

[**Random media Discord bot**](https://github.com/Electronic-Mango/random-media-discord-bot) is stored in `./random-media-discord-bot/config/` subdirectory.
It consists of two files:

 * `custom_settings.yml` - custom parameters of the bot, similarly to Reddit API API
 * `custom_sources.yml` - file with sources of memes and copypastas to send by the bot

Both files are not stored in the Docker image, but in containers mounted volume.
`docker-compose.yml` configures `CUSTOM_SETTINGS_PATH` environment variable to point to correct settings YAML.
Path to `custom_sources.yml` is defined at the end of `custom_settings.yml`.

There's only one parameter needed to run the bot - its token.
It can be configured in `./random-media-discord-bot/config/custom_settings.yml`:

```yaml
bot:
  # Fill with your Discord bot token
  token: <discord-bot-token>
```

`custom_settings.yml` also contains configuration for command (and command group) names and descriptions, to better match the fact that it's going to be sending back memes and copypastas.

Other parameters will use their default values stores in `settings.yml` file in Random media Discord bot itself.

Both memes and copypastas are loaded from reddit submissions.
You can check `custom_sources.yml` whether it fits your preference for subreddits to read data from.

`media` section defines sources for images loaded via `/meme get` command.
`text` section defines sources for copypastas loaded via `/copypasta get` command.
You can change subreddits by supplying new entries, or modifying existing ones, and just change subreddit name in the URL.
More detailed description of API endpoints is available in [Reddit API API project page](https://github.com/Electronic-Mango/reddit-api-api).

More detailed description of settings YAML and sources YAML is available in [Random media Discord bot](https://github.com/Electronic-Mango/random-media-discord-bot) project and its default YAML files.