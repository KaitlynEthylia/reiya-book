# Scripting

> This section assumes some understanding of Lua. If you're not familiar with
> the language, start with
> [Programming In Lua](https://lua.org/pil/contents.html).

Reiya provides an interface for automating granular organisation of feeds and
posts via [Lua](https://lua.org) scripting.

Lua scripts should be stored in the `$REIYA_CONFIG_DIR/lua` folder, to which
[`package.path`](https://lua.org/manual/5.4/manual.html#pdf-package.path) points
by default.

Every feed can have an associated module for each action. It also has an `init`
script which is run when Reiya first loads. The init script gives no input and
expects no output, but provides access to the Lua api, allowing you to query it
for external tasks.

The global configuration also has modules for these actions, these apply to any
feeds that don't have them set by default.

Feeds can explicitly disable their action to opt out of the default.

See: [`conifg.toml` / `feeds.toml`](../2-config/options.md#the-scripts-section)

## Actions

An action is a function that get's called by Reiya at a specific time, and can
provide certain outputs to change Reiya's behaviour. Actions also have full
access to Reiya's [Lua API](./4-api/lua.md) to make other modifications and
queries.

To define an action, create a lua file and return a function:

```lua
-- $REIYA_CONFIG_DIR/lua/read.lua
-- This action is called whenever we read a post.

return function(content, feed, targs, marks)
  -- Do something
end
```

The actions currently available are:

- [`new`](#new)
- [`transform`](#transform)
- [`read`](#read)
- [`mark`](#mark)
- [`download`](#download)

## `new`

The `new` action runs whenever Reiya recieves a new post. As input it is given
the parameters: `content`, `feed`, and `marks`.

- `content`: The content of the post as a string.
- `feed`: An identifier for the feed this post came from. Can be used with api
  functions such as `reiya.get_tags(feed)`.
- `marks`: See [Marks](#marks-table).

As output, `new` can optionally return `notify`, and `marks`.

`notify` is a boolean value determining whether to send a notification about the
post. If `nil` is returned in it's place, the default value will be used.

`new` is able to be set to rerun_on_change, meaning that if the script is
changed, it will be run for all posts that Reiya already knows about.

## `transform`

The transform action allows a script to modify the contents of a post before it
is read, such as filtering out certain keywords, or transforming it from HTML to
markdown.

See [Example: HTML To Markdown](./examples/html-to-markdown.md)

As input it receives: `content`, `mime`, `feed`, and `marks`.

- `content`: The content of the post as a string.
- `mime`: The mime type of the content, if one can be determined, otherwise
  `nil`.
- `feed`: An identifier for the feed this post came from. Can be used with api
  functions such as `reiya.get_tags(feed)`.
- `marks`: See [Marks](#marks-table).

As output, it expects a string which will be the content of the post shown to
the reader.

> When the `transform` action is called is not guaranteed. All that is
> guranateed is that the transformed content will be shown to the user. You
> should use `read` or `new` if you want to perform side-effects.

> [TODO]
> explain other actions

## Marks

All actions in lua take a table called `marks` as a parameter. This table is
reference and is explicitly allowed to be modified. It contains, as a set, the
marks that the post currently has.

Note that as it is a set, it is of type `type<string, boolean>`, and NOT
`srting[]`.

Setting a mark in the set to `nil` will preserve whether or not it is currently
present. Setting it to `false` will remove it (if present), and setting it to
`true` will add it (if not present).

Actions that take marks as a return allow you to overwrite the marks set with a
new table. Returning `nil` for marks, will preserve the old table, but you can
alternatively return `{}` (An empty table), or any other table that isn't the
`marks` parameter, in order to change the posts marks from scratch
