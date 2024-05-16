# Explicit Word Filter

```lua
-- $REIYA_CONFIG_DIR/lua/new.lua

return function(content, _, marks)
  for _,v in ipairs { 'list', 'of', 'unpleasent', 'words' } do
    -- Check if any of the words in our list are present in the content
    if content:match(v) then
      -- Add the 'explicit' mark to the post
      marks['explicit'] = true
      break
    end
  end

  -- No need to return anything because `marks` is a reference
end
