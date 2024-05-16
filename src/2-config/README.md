# Configuration

Configuring Reiya is primarily done through two toml files: `config.toml`, and
`feeds.toml`. The settings set here can also be changed on-the-fly via the
API[^1].

- `config.toml` is used for global settings that.

- `feeds.toml` contains the RSS feeds you want to collate, as well as settings
  that apply to those feeds, overwriting global settings.

There are also a handful of environment variables to configure Reiya;
specifically, the locations where Reyia may store files:

- `$REIYA_CONFIG_DIR` is the folder in which all configuration files and
  scripts will live. It is the parent directory of both `config.toml` and
  `feeds.toml`.

  By default it will be set to `$XDG_CONFIG_HOME/reiya/`. If that is not set, it
  will fallback to `~/.config/reiya`.

- `$REIYA_CACHE_DIR` is the folder in which chache files will be stored.

  By default it will be set to `$XDG_CACHE_HOME/reiya`, falling back to
  `~/.cache/reiya` if not set.

[^1]: Dynamically changing settings via the API requires the "dynamic-settings"
      feature to be enabled when compiling from source. (It is by default.)

