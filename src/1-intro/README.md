# Introduction

> [NOTE]
>
> Anything in these NOTE boxes will be gone when this is actually finished, this
> is just for the people I've asked for feedback on this. :3

## What Is Reiya?

[Reiya](https://github.com/KaitlynEthylia/reiya) is an 'RSS-Reader Backend'
designed with the goal of being *versatile*, *configurable*, and *portable*.

### Versatile:

Reiya  provides a [documented](../4-api/ipc.md) API for managing and accessing
content, meaning the same feeds with the same bookmarks can be viewed in a
number of different formats, or even from a different PC entirely (with
port-forwarding).

> [NOTE]
>
> I plan to also make a CLI and TUI frontend for reyia by the time I actually
> release this. Maybe a GUI one someday but that's more difficult and less
> important to me.

### Configurable

Reiya provides plenty of configuration options for individually managing RSS
feeds, as well as scripting facilities via the [Lua](https://lua.org)
programming language that enable you to transform and filter content, organise
and collect posts, and run external tasks, all automatically.

### Portable

Reiya can be configured entirely from plain old text files (`.toml` and `.lua`
files specifically), including your settings, your feeds, and all your
automation, meaning you can move it from one PC to another without any extra
work.

> [NOTE]
>
> This is only special because I also want to target windows. :P

## 'RSS-Reader Backend'?

Reiya does not provide any facilities for viewing content from RSS feeds. It
runs as a background process that can collect and poll RSS feeds and send
notifications for new content, and provides an API for retrieving, organising,
and downloading content.

To use Reyia, you need to connect to it from a frontend:

> [TODO]
>
> Link to frontends here.
