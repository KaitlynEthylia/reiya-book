# API Reference

Reyia provides two different APIs for interacting with it.

- The [Lua API](./lua.md) provides a series of functions for getting extra
  information and modifying Reiya from within Lua scripts. This API is only
  accessible to Lua scripts run through Reyia.

  To commincate with Reiya from external Lua scripts, use the Socket API via the
  [LuaSocket](https://github.com/lunarmodules/luasocket) library, or `io.popen`
  on an instance of `netcat(1)`.

- The [Socket API](./socket.md) allows external applications to communicate
  with Reyia over TCP, enabling the creation of different frontends.
