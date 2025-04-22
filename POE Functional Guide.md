---
type: Page
title: POE Functional Guide
description: null
icon: null
createdAt: '2025-04-22T14:46:36.633Z'
creationDate: 2025-04-22 16:46
modificationDate: 2025-04-22 17:03
tags: []
coverImage: null
---

The full "Functional Guides" page is much longer than a single response will allow, so I'll stream it to you in several numbered chunks, **verbatim** and converted 1‑for‑1 to CommonMark.

Here is **Part 1 / n** (everything from the top of the page through the end of the "Accessing other bots on Poe" section, including the complete code blocks).

```markdown
# Functional Guides
*Updated 20 days ago | Library version **v1.0***
---
If you are just getting started with server bots, we recommend checking
out our [quick start](https://creator.poe.com/docs/quick-start) guide.
The following guides provide examples of accomplishing specific things
with your bot.
The examples assume that you have installed the latest version of
`fastapi_poe` (you can install this with `pip install fastapi_poe`).
The full code examples also assume that you are hosting your bot on
**Modal**. Although we recommend Modal for its simplicity, you should be
able to deploy your server bot on *any* cloud provider. To learn how to
set up Modal, please follow Steps 1 and 2 in our Quick start. If you
already have Modal set up, simply copy the full code examples into a file
called `main.py` and then run
```

modal deploy main.py

```text
Modal will deploy your bot server to the cloud and output the server
URL. Use that URL when creating a server bot on Poe.
---
## Accessing other bots on Poe
The Poe **Bot Query API** allows creators to invoke other bots on Poe
(which includes Poe‑created bots like GPT‑3.5‑Turbo and Claude‑Instant
and bots created by other creators). This access is **free** so that
creators don't have to worry about LLM costs. For every user message,
server‑bot creators may make **up to 10 calls** to another bot of their
choice.
### Declare dependency in your `PoeBot` class
You have to declare your bot dependencies using the **settings** endpoint.
```python
async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
    return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 1})
```

### Forward the user's query

In your `get_response` handler, use the `stream_request` function to

invoke any bot you want. The following is an example where we forward

the user's query to **GPT‑3.5‑Turbo** and return the result.

```python
async def get_response(
    self, request: fp.QueryRequest
) -> AsyncIterable[fp.PartialResponse]:
    async for msg in fp.stream_request(
        request, "GPT-3.5-Turbo", request.access_key
    ):
        yield msg
```

### Full example

```python
class GPT35TurboBot(fp.PoeBot):
    async def get_response(
        self, request: fp.QueryRequest
    ) -> AsyncIterable[fp.PartialResponse]:
        async for msg in fp.stream_request(
            request, "GPT-3.5-Turbo", request.access_key
        ):
            # Add whatever logic you'd like here before yielding the result!
            yield msg
    async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
        return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 1})
```

```python
from __future__ import annotations
from typing import AsyncIterable
from modal import App, Image, asgi_app
import fastapi_poe as fp
class GPT35TurboBot(fp.PoeBot):
    async def get_response(
        self, request: fp.QueryRequest
    ) -> AsyncIterable[fp.PartialResponse]:
        async for msg in fp.stream_request(
            request, "GPT-3.5-Turbo", request.access_key
        ):
            # Add whatever logic you'd like here before yielding the result!
            yield msg
    async def get_settings(self, setting: fp.SettingsRequest) -> fp.SettingsResponse:
        return fp.SettingsResponse(server_bot_dependencies={"GPT-3.5-Turbo": 1})

REQUIREMENTS = ["fastapi-poe==0.0.48"]
image = Image.debian_slim().pip_install(*REQUIREMENTS)
app = App("gpt35turbo-poe")

@app.function(image=image)
@asgi_app()
def fastapi_app():
    bot = GPT35TurboBot()
    # see https://creator.poe.com/docs/quick-start#configuring-the-access-credentials
    app = fp.make_app(bot, access_key=<YOUR_ACCESS_KEY>, bot_name=<YOUR_BOT_NAME>)
    return app
```

That's it! Try chatting with your bot now, and you should see responses

from GPT‑3.5‑Turbo.

> 🚧 **Warning**

If you see some error related to bot call counts, it's possible your

dependencies are not updated properly. See

"Updating Bot Settings" for possible resolutions.

```text
---
Reply with "part2" (or anything similar) whenever you're ready for the
next chunk, which will start with **"Accessing a media generation bot on Poe"** and continue onward.
```