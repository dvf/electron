# ⚡️electron

Rapid RPC Framework for your Python services using Asyncio + [MsgPack](https://msgpack.org/index.html)

## Installation

```
$ pip install electron-rpc
```

## How does it work?

Create a server instance with a single endpoint `sum()` which outputs the sum of two values:

```python
from electron.server import Server

app = Server()


@app.endpoint("sum")
async def sum(a, b, **kwargs):
    return f"Result: {a + b}"


app.run()
```

```
        __          __                      
  ___  / /__  _____/ /__________  ____      
 / _ \/ / _ \/ ___/ __/ ___/ __ \/ __ \     
/  __/ /  __/ /__/ /_/ /  / /_/ / / / /     
\___/_/\___/\___/\__/_/   \____/_/ /_/      

⚡ electron build v0.0.1-beta                              

Listening on 127.0.0.1:9999

Registered Endpoints:
- sum
```

## Calling the endpoint from a client

electron uses MsgPack-RPC, so craft a payload and send it over TCP:

```python
import socket

from electron.messages import RemoteProcedureCall

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

sock.connect(("127.0.0.1", 9999))
sock.send(RemoteProcedureCall(endpoint="sum", args=[1, 2]).encode())

data = sock.recv(1024)
print(f"Received: \n{data.decode()}")
sock.close()
```
```
Received: 
Result: 3
```
