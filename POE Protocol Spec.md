---
type: Page
title: POE Protocol Spec
description: null
icon: null
createdAt: '2025-04-22T14:27:56.564Z'
creationDate: 2025-04-22 16:27
modificationDate: 2025-04-22 16:28
tags: []
coverImage: null
---

[Poe](https://poe.com/) is a platform for interacting with AI-based bots. Poe provides access to popular chat bots like OpenAI's GPT-3.5-Turbo and Anthropic's Claude, but also allows a creator to create their own bot by implementing the following protocol.

Introduction

[https://creator.poe.com/docs/poe-protocol-specification#introduction](https://creator.poe.com/docs/poe-protocol-specification#introduction)

This specification provides a way to run custom bots from any web-accessible service that can be used by the Poe app. Poe users will send requests to the Poe server, which will in turn send requests to the bot server using this specification. As the bot server responds, Poe will show the response to the user. See [Quick start](https://creator.poe.com/docs/quick-start) for a high-level introduction to running a server bot.

Terminology

[https://creator.poe.com/docs/poe-protocol-specification#terminology](https://creator.poe.com/docs/poe-protocol-specification#terminology)

- ***Poe server****:* server run by Poe that receives client requests, turns them into requests to bot servers, and streams the response back to the Poe client.

- ***Bot server****:* server run by the creator that responds to requests from Poe servers. The responses are ultimately shown to users in their Poe client.

Concepts

[https://creator.poe.com/docs/poe-protocol-specification#concepts](https://creator.poe.com/docs/poe-protocol-specification#concepts)

Identifiers

[https://creator.poe.com/docs/poe-protocol-specification#identifiers](https://creator.poe.com/docs/poe-protocol-specification#identifiers)

The protocol uses identifiers for certain request fields. These are labeled as "identifier" in the specification. Identifiers are globally unique. They consist of a sequence of 1 to 3 lowercase ASCII characters, followed by a hyphen, followed by 32 lowercase alphanumeric ASCII characters or `=` characters (i.e., they fulfill the regex `^[a-z]{1,3}-[a-z0-9=]{32}$`).

The characters before the hyphen are a tag that represents the type of the object. The following types are currently in use:

- `m`: represents a message

- `u`: represents a user

- `c`: represents a conversation (thread)

- `d`: represents metadata sent with a message

Authentication

[https://creator.poe.com/docs/poe-protocol-specification#authentication](https://creator.poe.com/docs/poe-protocol-specification#authentication)

While creating a bot, a creator can provide an access key consisting of 32 ASCII characters. To allow bot servers to confirm that the requests come from Poe, all requests will have an Authorization HTTP header `Bearer <access_key>`.

Content types

[https://creator.poe.com/docs/poe-protocol-specification#content-types](https://creator.poe.com/docs/poe-protocol-specification#content-types)

Messages may use the following content types:

- `text/plain`: Plain text, rendered without further processing

- `text/markdown`: Markdown text. Specifically, this supports all features of GitHub-Flavored Markdown (GFM, specified at [https://github.github.com/gfm/](https://github.github.com/gfm/)). Poe may however modify the rendered Markdown for security or usability reasons.

Versioning

[https://creator.poe.com/docs/poe-protocol-specification#versioning](https://creator.poe.com/docs/poe-protocol-specification#versioning)

It is expected that the API will be extended in the future to support additional features. The protocol version string consists of two numbers (e.g., 1.0).

Response

[https://creator.poe.com/docs/poe-protocol-specification#response-2](https://creator.poe.com/docs/poe-protocol-specification#response-2)

The server's response is ignored.

report_reaction

[https://creator.poe.com/docs/poe-protocol-specification#report_reaction](https://creator.poe.com/docs/poe-protocol-specification#report_reaction)

This request takes the following additional parameters:

- `message_id` (identifier with type `m`): The message for which a reaction is being provided.

- `user_id` (identifier with type `u`): The user providing the reaction.

- `conversation_id` (identifier with type `c`): The conversation giving rise to the reaction.

- `reaction` (string): May be `like`, `dislike`, `heart`, `laughing`, `surprised`, `sad`. Additional values may be added in the future; bot servers should ignore reactions with values they do not recognize.

Response

[https://creator.poe.com/docs/poe-protocol-specification#response-3](https://creator.poe.com/docs/poe-protocol-specification#response-3)

The server's response is ignored.

report_error

[https://creator.poe.com/docs/poe-protocol-specification#report_error](https://creator.poe.com/docs/poe-protocol-specification#report_error)

When the bot server fails to use the protocol correctly (e.g., when it uses the wrong types in response to a `settings` request, the Poe server may make a `report_error` request to the server that reports what went wrong. The protocol does not guarantee that the endpoint will be called for every error; it is merely intended as a convenience to help bot server creators debug their bot.

This request takes the following additional parameters:

- `message` (string): A string describing the error.

- `metadata` (dictionary): May contain metadata that may be helpful in diagnosing the error, such as the `conversation_id` for the conversation. The exact contents of the dictionary are not specified and may change at any time.

Response

[https://creator.poe.com/docs/poe-protocol-specification#response-4](https://creator.poe.com/docs/poe-protocol-specification#response-4)

The server's response is ignored.

Samples

[https://creator.poe.com/docs/poe-protocol-specification#samples](https://creator.poe.com/docs/poe-protocol-specification#samples)

![Untitled](https://files.readme.io/5f4f294-image.png)

Suppose we're having the above conversation over Poe with a bot server running at `https://ai.example.com/llm`.

For the Poe conversation above, the Poe server sends a POST request to `https://ai.example.com/llm` with the following JSON in the body:

JSON

`{     "version": "1.0",     "type": "query",     "query": [         {             "role": "user",             "content": "What is the capital of Nepal?",             "content_type": "text/markdown",             "timestamp": 1678299819427621,         }     ],     "user": "u-1234abcd5678efgh",     "conversation": "c-jklm9012nopq3456", }`

The bot server responds with an HTTP 200 response code, then sends the following server-sent events:

JSON

`event: meta data: {"content_type": "text/markdown", "linkify": true}  event: text data: {"text": "The"}  event: text data: {"text": " capital of Nepal is"}  event: text data: {"text": " Kathmandu."}  event: done data: {}`

The text may also be split into more or fewer individual events as desired. Sending more events means that users will see partial responses from the bot server faster.

Next steps

[https://creator.poe.com/docs/poe-protocol-specification#next-steps](https://creator.poe.com/docs/poe-protocol-specification#next-steps)

- Check out our [quick start](https://creator.poe.com/docs/quick-start) to promptly get a bot running.

- [fastapi-poe](https://github.com/poe-platform/fastapi_poe), a library for building Poe bots using the FastAPI framework. We recommend using this library if you are building your own bot.

- [Example Bots](https://creator.poe.com/docs/examples)