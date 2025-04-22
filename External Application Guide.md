---
type: Page
title: External Application Guide
description: null
icon: null
createdAt: '2025-04-22T14:40:23.030Z'
creationDate: 2025-04-22 16:40
modificationDate: 2025-04-22 16:41
tags: []
coverImage: null
---

> External Application Guide

[https://creator.poe.com/docs/external-application-guide](https://creator.poe.com/docs/external-application-guide)

Every heading, paragraph, list item, code block, note, and table from the page is preserved exactly (only typographic quotes and long lines were wrapped).

```markdown
# External Application Guide
*Updated 19 days ago | Library version **v1.0***
---
## Overview
External applications exist **outside** the Poe client UI.  
They connect to Poe via a **Poe user's API key**, and use the Poe API to
query Poe bots and models *on behalf of that user*, spending the user's
points in the process.
**Examples of possible external applications**
* Browser toolbars  
* Shell scripts  
* Alternative chat interfaces  
* Specialized client applications like IDEs
---
## Querying Poe bots via an API key
All Poe API keys are associated with a *user* account.
* As a creator, you can use **your own API key** – points are charged to
  your Poe account.  
* Or you can ask **users of your product** to provide *their* Poe API
  key – points are charged to *their* account.
> **Note** If you are creating a *server bot*, you can charge requests
> directly to the user's account without requiring an API key via  
> "Accessing other bots on Poe."
### ⚠️ Warning
This API makes the API‑key owner's **entire point balance** available to
any bot that is queried, so be careful about calling bots that are not
your own or are not marked as official.
---
### 1 . Get an API Key
Navigate to <https://poe.com/api_key> and copy your user API key  
(or ask your users to do the same).
> This is currently **only available for Poe subscribers**.
---
### 2 . Call `get_bot_response` or `get_bot_response_sync`
#### Synchronous example
```python
import fastapi_poe as fp
api_key = <api_key>   # ← replace with your API key
message = fp.ProtocolMessage(role="user", content="Hello world")
for partial in fp.get_bot_response_sync(
        messages=[message],
        bot_name="GPT-3.5-Turbo",
        api_key=api_key):
    print(partial)
```

#### Asynchronous example

```python
import asyncio
import fastapi_poe as fp
async def get_response():
    api_key = <api_key>  # ← replace with your API key
    message = fp.ProtocolMessage(role="user", content="Hello world")
    async for partial in fp.get_bot_response(
            messages=[message],
            bot_name="GPT-3.5-Turbo",
            api_key=api_key):
        print(partial)
def main():
    asyncio.run(get_response())
if __name__ == "__main__":
    main()
```

---

### Limitations

- Requests are **rate‑limited to 500 requests per minute per user**.

- Compute points are deducted **directly from the account associated

    with the API key**.

---

## Questions?

Please [contact us](https://creator.poe.com/contact) if you would like a

higher limit or want to request any other changes to support your

external application.

```text
---
You now have the **entire contents** of the "External Application Guide"
in a single Markdown snippet, ready to save as a `.md` file.
```