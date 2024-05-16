# Config Options

> [NOTE]
>
> There will probably be more configuration options in the future, still trying
> to work out what thing seem like they should be included.
>
> Already considering adding an option for a systray icon and such.

The `config.toml` file contains global configuration for Reiya. Most of these
settings can be overwritten by [individual feeds](./feeds.md), but act as defaults.

There are no required entries in `config.toml`. (The entire file is not required.)

The available configuration options are:

- [`persist`](#the-persist-field)
- [`notify`](#the-notify-field)
- [`poll`](#the-poll-field)
- [`anchor`](#the-anchor-field)
- [`[download]`](#the-download-section)
  - [`path`](#the-path-field)
  - [`auto`](#the-auto-field)
  - [`ensure`](#the-ensure-field)
  - [`mode`](#the-mode-field)
- [`[scripts]`](#the-scripts-section)
  - [`join`](#the-join-field)

## The `persist` field

`persist`[^1] tells Reiya how to handle dynamic changes to the settings.

Is allowed to be either the strings "Never", "Always", or "Allow". Setting it to
`true` or `false` is also allowed, corresponding to "Always", and "Never"
respectively.

Default: "Allow"

- "Never": Any changes made dynamically via API calls will be discarded when the
  Reiya is restarted.

- "Allow": Any changes are discarded when Reiya is restarted unless they
  explicitally request to be persisted.

- "Always": All dynamic changes to settings are persisted by default, they will
  only be temporary if they explicitally request to be.

## The `notify` field

Whether to send a notification for new posts. This can be configured on a
per-feed basis, and even more granuarly within Lua scripts. See:
[Example: Notification Filter](../3-scripting/examples/notifications.md)

Default: false

> [NOTE]
>
> I'll probably add some string options here to specify how to send
> notifications, like "libnotify", "dbus", "windows-toast", etc.

## The `poll` field

The polling rate tells Reiya how frequently to request updates to the RSS feed.
This doesn't guarantee when requests will be made, only how frequently.

Setting poll to the empty string will cause it to only be polled if Reiya does
not already have a copy of the RSS feed.

Default: 1 hour

> [NOTE]
>
> I'll add details about specific format once I've actually got it implemented

## The `anchor` field

The anchor sets a specific point in time to anchor the polling rate to, allowing
you to be specific about when a feed should be polled.

> [NOTE]
>
> I don't know how useful this would be? If it ends up being a pain to
> implement, ill probably consider removing it. Again, specific format details
> later.

## The `[download]` section

The download settings control how Reiya will act when downloading content from
a feed. If the content of the feed is an external link (i.e. a video or music
file), you can request to follow the link and download that content, or you can
set it to do so automatically.

```toml
[download]
path = "~/Downloads/RSS"
auto = "Read"
ensure = false
mode = "All"
```

The entire section is optional, but for downloading to be enabled,
[`path`](#the-path-field) must at least be provided.

### The `path` field

The path to the folder in which Reiya should save downloaded content.

Default: None

When content is downloaded, it is saved in a subdirectory of the download path
corresponding to the feed from whence it came.

If a download path is not specified, there is no fallback. Attempting to
download without a valid download path will fail.

### The `auto` field

Whether or not to automatically download new posts.

This can be set to any of: "Never", "Read", or "Arrive". It can alternatively be
set to a boolean, with `true` corresponding to "Arrive", and `false`
corresponding to "Never".

Default: "Never"

- "Never": Never automatically download new posts. Specific posts can still be
  downloaded via API requst.

- "Read": New posts will be automatically downloaded once they have been read.
  (Once their content has been requested by the API).

- "Arrive": New posts will be automatically downloaded as soon as the RSS feed
  is updated and polled.

### The `ensure` field

When Reiya launches, ensure that certain files are downloaded, and if not,
download them. This can be either `true` or `false`.

Default: `false`

If [`auto`](#the-auto-field) is set to "Never". This will do nothing. if it is
set to "Read". It will only check posts that are marked as read.

### The `mode` field

This tells Reiya how to handle requests to download posts that don't have
external content. It can be set to "Ignore", "Save", "RSS".

Default: "Ignore"

> [NOTE]
>
> I don't really like the names of these options, but I couldn't come up with
> anything better.

- "Ignore": Do nothing, requests to download this post won't download anything.

- "Save": Requesting to download the post will save the content of the post to
  it's own file.

- "Feed": Requesting to download a post that has no file will save the feed's
  raw RSS file to the downloads folder. This file is not duplicated.

## The `[scripts]` section

The scripts sections gives Reiya information about the Lua scripts associated
with certain actions.

Each item is a string, giving the name of the Lua module containing the code for
the action. Optionally, actions that support it may be a table or a table with
a `module` field, and a `rerun_on_change` boolean field, defaulting to false.

Only some actions support setting `rerun_on_change`.

See [Scripting](../3-scripting) for more information.

The example below shows every field with it's default values.

```toml
[scripts]
init = ""
new = "new"
download = "download"
transform = { module = "transform", rerun_on_change = false }
mark = { module = "mark", rerun_on_change = false }
read = { module = "read", rerun_on_change = false }
join = false
```

### The `join` field

`join` is a special field in the `[scripts]` sections. By enabling script
joining, if a feed sets a specific lua module to an action. That feed's module
will be run, but the global run will also be run. The outputs of the action
will be taken from the feed-specific module, but any side effects of the global
one will still occur.

Default: `false`

[^1]: Only available when compiled with the "dynamic-settings" feature. If this
      feature is not available, the setting will be ignored but will not error.
