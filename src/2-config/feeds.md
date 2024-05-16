# TODO

The `feeds.toml` file contains the list of RSS feeds to collate, as well as
specific settings for those feeds.

Every root level item in `feeds.toml` should be a table, it's name being the
name you want to give to the feed.

Each feed has the following configuration options, with required one's marked by
an asterisk (*):

- [`url`*](#the-url-field)
- [`follow`*](#the-follow-field)
- [`tags`](#the-tags-field)
- [`marks`](#the-marks-field)
- [`notify`](./options.md#the-notify-field)
- [`poll`](./options.md#the-poll-field)
- [`anchor`](./options.md#the-anchor-field)
- [`[download]`](./options.md#the-download-section)
- [`[scripts]`](./options.md#the-scripts-section)

## The `url` field

The URL of the RSS feed. This can either be a http(s) path, or a file path.

File paths are not watched for changes, meaning they obey the polling rate just
the same as online paths. They also interact with downloads in the exact same
way.

## The `follow` field

Whether or not to get a post's content by following a link it provides. This is
useful RSS feeds that are just stubs that link to websites, or for podcasts and
the like that provide a link to an audio/video file.

Alternatively you can provide a string, describing the XML tag and property on
which the link is found. For example, RSS feeds for itunes, instead of storing
thieir links in the `<link>` tag, will put them in the `url` property of an
`enclosure` field. in which case you should set:

```toml
follow = "<enclosure>:url"
```

Default: false

## The `tags` field

A list of tags to associate with the feed. Tags are the primary way of
organising feeds.

Default: `[]`

## The `marks` field

A list of marks to automatically apply to posts from a feed. Marks are the
primary way of filtering and organising individual posts.

Default: `[]`

You can also use a script to determine marks to automatically apply to
individual posts. See [Example: Explicit Content
Filter](../3-scripting/examples/explicit.md)

## Example

Here's an example `feeds.toml`, based on my own personal usage

```toml
["Reverse Trivia"]
url = "https://techdif.co.uk/podcast/feed.xml"
poll = ""
follow = "<enclosure>:url"
tags = ["podcast", "explicit"]
download.path = "~/Media/techdif/"
download.auto = true
download.ensure = true

["Lingthusiasm"]
url = "http://feeds.soundcloud.com/users/soundcloud:users:237055046/sounds.rss"
follow = "<enclosure>:url"
tags = ["podcast"]

["Talia's Blog"]
url = "https://blog.but.gay/feeds/all.rss.xml"
tags = ["blog"]

["My Blog"]
url = "https://girl.but.gay/rss.xml"
notify = false
tags = ["blog"]
```
