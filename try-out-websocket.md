# Try out Websockets

## Using `fastapi`

```bash
# create server
uv run --with fastapi --with "uvicorn[standard]" python -c "
import uvicorn
from fastapi import FastAPI, WebSocket

app = FastAPI()

@app.websocket('/ws/echo')
async def echo(ws: WebSocket):
    await ws.accept()
    try:
        while True:
            msg = await ws.receive_text()
            await ws.send_text('You send: ' + msg)
    except Exception:
        pass

uvicorn.run(app, host='localhost', port=8000)
"

# (in another teminal) start a conversation with the server
wscat -c ws://localhost:8000/ws/echo

# Prerequisites:
# - uv (brew install uv)
# - wscat (npm install -g wscat)
```


## Using `websockets`

```bash
# create server
uv run --with websockets python -c "
import asyncio, websockets
async def echo(ws):
    async for msg in ws:
        await ws.send('You send: '+msg)
async def main():
    async with websockets.serve(echo, 'localhost', 8000):
        await asyncio.Future()
asyncio.run(main())
"

# start a conversation with the server
wscat -c ws://localhost:8000/ws/echo

# Prerequisites:
# - uv (brew install uv)
# - wscat (npm install -g wscat)
```

