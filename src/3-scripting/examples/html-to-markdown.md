# HTML To Markdown

```lua
-- $REIYA_CONFIG_DIR/lua/transform.lua

return function(content, mime)
  if mime == "text/html" then
    -- Create a temporary file to write the html content to
    local path = os.tmpname()
    local h = io.open(path, 'h')

    -- If that failed then just return the HTML content
    if not h then return end

    -- Write the HTML content to the temporary file and close it.
    h:write(content)
    h:close()

    -- Run pandoc to convert the html file to markodwn
    local ph = io.popen('pandoc --from html --to markdown ' .. n)

    -- Again, just return HTML if it fails
    if not ph then return end

    -- Read the stdout from pandoc to get the result
    local result = ph:read 'a'
    ph:close()
    return result
  end
end
```
