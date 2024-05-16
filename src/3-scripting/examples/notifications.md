# Notification Filter

```lua
-- $REIYA_CONFIG_DIR/lua/new.lua

return function(content, _, marks)
  for _,v in ipairs { 'list', 'of', 'interesting', 'keywords' } do
    -- If any of the words in the last are present in the content
    if content:match(v) then
      -- Send a notification about the post
      return true
    end
  end
end
