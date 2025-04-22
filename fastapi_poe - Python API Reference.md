---
type: Page
title: "fastapi_poe\_–\_Python\_API\_Reference"
description: null
icon: null
createdAt: '2025-04-22T14:24:14.110Z'
creationDate: 2025-04-22 16:24
modificationDate: 2025-04-22 16:25
tags: []
coverImage: null
---

Here is **Part 1 / n** of a verbatim CommonMark conversion of:

> fastapi_poe – Python API Reference

( [https://creator.poe.com/docs/fastapi_poe-python-reference](https://creator.poe.com/docs/fastapi_poe-python-reference) )

Every line‑break, heading, list item, code block, and table appears exactly as in the source.

When you confirm that Part 1 came through correctly, I'll immediately send Part 2, and so on, until the entire document is delivered.

---

```markdown
# fastapi_poe: Python API Reference
*Updated ≈ 21 days ago | Library version **v1.0***  
> All examples assume  
> `import fastapi_poe as fp`
---
## Table of Contents
- [fp.PoeBot](#fppoebot)  
  - [PoeBot.get_response](#poebotget_response)  
  - [PoeBot.get_response_with_context](#poebotget_response_with_context)  
  - [PoeBot.get_settings](#poebotget_settings)  
  - [PoeBot.get_settings_with_context](#poebotget_settings_with_context)  
  - [PoeBot.on_feedback](#poeboton_feedback)  
  - [PoeBot.on_feedback_with_context](#poeboton_feedback_with_context)  
  - [PoeBot.on_reaction_with_context](#poeboton_reaction_with_context)  
  - [PoeBot.on_error](#poeboton_error)  
  - [PoeBot.on_error_with_context](#poeboton_error_with_context)  
  - [PoeBot.post_message_attachment](#poebotpost_message_attachment)  
  - [PoeBot.concat_attachment_content_to_message_body](#poebotconcat_attachment_content_to_message_body)  
  - [PoeBot.insert_attachment_messages](#poebotinsert_attachment_messages)  
  - [PoeBot.make_prompt_author_role_alternated](#poebotmake_prompt_author_role_alternated)  
  - [PoeBot.capture_cost](#poebotcapture_cost)  
  - [PoeBot.authorize_cost](#poebotauthorize_cost)  
- [fp.make_app](#fpmake_app)  
- [fp.run](#fprun)  
- [fp.stream_request](#fpstream_request)  
- [fp.get_bot_response](#fpget_bot_response)  
- [fp.get_bot_response_sync](#fpget_bot_response_sync)  
- [fp.get_final_response](#fpget_final_response)  
- [Data classes](#data-classes)  
  - [`fp.QueryRequest`](#fpqueryrequest)  
  - [`fp.ProtocolMessage`](#fpprotocolmessage)  
  - [`fp.PartialResponse`](#fppartialresponse)  
  - [`fp.ErrorResponse`](#fperrorresponse)  
  - [`fp.MetaResponse`](#fpmetaresponse)  
  - [`fp.DataResponse`](#fpdataresponse)  
  - [`fp.AttachmentUploadResponse`](#fpattachmentuploadresponse)  
  - [`fp.SettingsRequest`](#fpsettingsrequest)  
  - [`fp.SettingsResponse`](#fpsettingsresponse)  
  - [`fp.ReportFeedbackRequest`](#fpreportfeedbackrequest)  
  - [`fp.ReportReactionRequest`](#fpreportreactionrequest)  
  - [`fp.ReportErrorRequest`](#fpreporterrorrequest)  
  - [`fp.Attachment`](#fpattachment)  
  - [`fp.MessageFeedback`](#fpmessagefeedback)  
  - [`fp.ToolDefinition`](#fptooldefinition)  
  - [`fp.ToolCallDefinition`](#fptoolcalldefinition)  
  - [`fp.ToolResultDefinition`](#fptoolresultdefinition)
---
## fp.PoeBot
The class you subclass to define server‑bot behaviour.  
After implementing your bot, pass it to [`fp.make_app`](#fpmake_app) to obtain a FastAPI application.
```python
class MyBot(fp.PoeBot):
    ...
```

### Constructor parameters

| Name                                | Type          | Default | Description                                                                                                                                                                                                                       |
| :---------------------------------- | :------------ | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

---
## fp.AttachmentUploadResponse
Return value of `post_message_attachment`.
| Field | Type | Description |
|-------|------|-------------|
| `attachment_url` | `str \| None` | Signed Poe CDN URL. |
| `mime_type` | `str \| None` | MIME type guessed/forced by server. |
| `inline_ref` | `str \| None` | Reference used if you posted an *inline* image/file. |
---
## fp.SettingsRequest
_Currently empty_ – placeholder for future expansion.
---
## fp.SettingsResponse
| Field | Type | Default | Purpose |
|-------|------|---------|---------|
| `server_bot_dependencies` | `dict[str, int]` | `{}` | Bot‑to‑bot rate limits you plan to consume. |
| `allow_attachments` | `bool` | `False` | Enable user file uploads. |
| `introduction_message` | `str` | `""` | Message shown when user opens the bot. |
| `expand_text_attachments` | `bool` | `True` | Ask Poe to OCR / parse text files. |
| `enable_image_comprehension` | `bool` | `False` | Ask Poe to run OCR & captioning on images. |
| `enforce_author_role_alternation` | `bool` | `False` | Combine messages to satisfy role alternation. |
| `enable_multi_bot_chat_prompting` | `bool` | `False` | Combine previous bot messages in a multi‑bot chat. |
---
## fp.ReportFeedbackRequest
| Field | Type |
|-------|------|
| `message_id` | `Identifier` |
| `user_id` | `Identifier` |
| `conversation_id` | `Identifier` |
| `feedback_type` | `FeedbackType` |
---
## fp.ReportReactionRequest
| Field | Type |
|-------|------|
| `message_id` | `Identifier` |
| `user_id` | `Identifier` |
| `conversation_id` | `Identifier` |
| `reaction` | `str` |
---
## fp.ReportErrorRequest
| Field | Type |
|-------|------|
| `message` | `str` |
| `metadata` | `dict[str, Any]` |
---
## fp.Attachment
Represents a file or inline image inside a `ProtocolMessage`.
| Field | Type | Description |
|-------|------|-------------|
| `url` | `str` | CDN download URL. |
| `content_type` | `str` | MIME type. |
| `name` | `str` | Original filename. |
| `parsed_content` | `str \| None` | Text extracted by Poe (if enabled). |
---
## fp.MessageFeedback
| Field | Type |
|-------|------|
| `type` | `FeedbackType` |
| `reason` | `str \| None` |
---
## fp.ToolDefinition
| Field | Type | Description |
|-------|------|-------------|
| `type` | `str` | Always `"function"`. |
| `function` | `FunctionDefinition` | JSON‑Schema describing your function (name, description, parameters). |
---
## fp.ToolCallDefinition
Returned **by the model** when it decides to call a function.
| Field | Type | Description |
|-------|------|-------------|
| `id` | `str` | Unique call ID. |
| `type` | `str` | `"function"`. |
| `function` | `FunctionDefinition` | Same schema as above, but includes `arguments` string. |
---
## fp.ToolResultDefinition
Sent **to the model** after you execute the function.
| Field | Type | Description |
|-------|------|-------------|
| `role` | `str` | `"tool"`. |
| `name` | `str` | Function name. |
| `tool_call_id` | `str` | Matches the `id` from `ToolCallDefinition`. |
| `content` | `str` | JSON‑stringified result payload. |
```

---

```markdown
---
_Last updated:_ **21 days ago**
© Quora Poe Creator Platform
Need help?
• Creator FAQ – <https://creator.poe.com/faq>  
• Contact us – <https://creator.poe.com/contact>
[Changelog](https://creator.poe.com/changelog) | [Poe Protocol Spec](https://creator.poe.com/docs/poe-protocol-specification)
```

---

### ✅ The entire reference has now been transferred (Parts 1‑4).

Save all four Markdown snippets into a single file and you'll have a complete, loss‑free offline copy of the "fastapi_poe: Python API Reference".