# Lua 101

study with the [TJ DevRies video](https://www.youtube.com/watch?v=CuWfgiwI73Q&list=PLep05UYkc6wTyBe7kPjQFWVXTlhKeQejM&index=3&ab_channel=TJDeVries)

## Background info

Lua is elegant.

- Lua uses "Mechanisms over Policies"
- Lua is designed to be embedded
- Lua is cool

## Comments

```lua
-- This is a comment. It starts with two dashes.

--[[
    This is also comment.
    But it spans multiple lines
--]]

```

## Variables: Simple Literals

```lua
local number = 5

local string = "hello world"
local singleu = 'also words'
local crazy = [[ This is
multi line and literal]]

local truth, lies = true, false

local nothing = nil
```

## Variables: Functions

```lua
local function hello(name)
  print("Hello", name)
end

local greet = function(name)
  -- .. is string concatenation
  print("Greetings, " .. name .. "!")
end
```

```lua
local higher_order = function(value)
  return function(another)
    return value + another
  end
end

local add_one = higher_order(1)
print("add_one(2) -> ", add_one((2)))
```

## Variables: Tables

Effectively, Lua's only data structure.

- Same structure is used for maps & lists.

As a list ...

```lua
local list = { "first", 2, false, function() print("Fourth!") end }
print("Yup, 1-indexed:", list[1])
print("Fourth is 4 ...:", list[4]())
```

As a map ...

```lua
local t = {
  literal_key = "a string",
  ["an expression"] = "also wrorks"
  [function() end] = t.rue
}

print("literal_key : ", t.literal_key)
print("an expression : ", t["an expression"])
print("function() end:", t[function() end])
```

## Variables: Not covered

- Thread
- Userdata

## Control Flow: `for`

```lua e
local favorite_accounts = { "teej_dv", "ThePrimeagen", "terminaldotshop" }
for index = 1, #favorite_accounts do
  print(index, favorite_accounts[index])
end

for index, value in ipairs(favorite_accounts) do
  print(index, value)
end
```

## Control Flow: `if`

```lua
local function action(loves_coffee)
  if loves_coffee then
    print("Check out `ssh terminal.shop` - it's cool!")
  else
    print("Check out `ssh terminal.shop` - it's still cool!")
  end
end

-- "falsey": nil, false
action() -- same as: action(nil)
action(false)

-- Everything else is "truthy"
action(true)
action(0)
action({})
```

## Modules

There isn't anything special about modules.
Modules are just files!

```lua
-- foo.lua
local M = {}
M.cool_function = function() end
return M
```

```lua
-- bar.lua
local foo = require('foo')
foo.cool_function()
```

## Functions: Multiple Returns

```lua
local returns_four_values = function()
  return 1, 2, 3, 4
end

first, second, last = returns_four_values()

print("first: ", first)
print("second: ", second)
print("last :", last)
-- the `4` is discarded :'(
```

```lua
local variables_arguments = function(...)
  local arguments = { ... }
  for i, v in ipairs({...}) do print(i, v) end
  return unpack(arguments)
end

print("==============")
print("1:", variables_arguments("hello", "world", "!"))
print("==============")
print("2:", variables_arguments("hello", "world", "!"), "<lost>")
```

## Functions: Calling

String Shorthand

```lua
local single_string = function(s)
  return s .. " - HOW!"
end

local x = single_string("hi")
local y = single_string "hi"
print(x, y)
```

Table Shorthand

```lua
local setup = function(opts)
  if opts.default == nil then
    opts.default = 17
  end

  print(opts.default, opts.other)
end

setup { default = 12, other = false }
setup { other = true }
```

## Functions: Colon Functions

```lua
local MyTable = {}

function MyTable.something(self, ...) end
function MyTable:something(...) end
```

## Metatables

```lua
local vector_mt = {}
vector_mt.__add = function(left, right)
  return setmetatable({
    left[1] + right[1],
    left[2] + right[2],
    left[3] + right[3],
  }, vector_mt)
end

local v1 = setmetatable({3, 1, 5}, vector_mt)
local v2 = setmetatable({-3, 2, 2}, vector_mt)
local v3 = v1 + v2
vim.print(v3[1], v3[2], v3[3])
vim.print(v3 + v3)
```

```lua
local fib_mt = {
__index = function(self, key)
    if key < 2 then return 1 end
  -- update the table, to save the intermediate results
    self[key] = self[key - 2] + self[key - 1]
    -- Return the results
    return self[key]
  end
}

local fib = setmetatable({}, fib_mt)

print(fib[5])
print(fib[1000])
```

## Quick Neovim Goodies

```lua
vim.keymap.set("n", "<space><space>x", "<cmd>source %<CR>")
vim.keymap.set("n", "<space>x", ":.lua<CR>")
vim.keymap.set("n", "<space>x", ">:lua<CR>")
```
