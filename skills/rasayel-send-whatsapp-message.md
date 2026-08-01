---
name: Send a WhatsApp message with Rasayel
description: Authenticate, find the channel and contact, then send a WhatsApp message via the Rasayel REST API.
api: openapi/rasayel-rest-openapi.yml
operations: [channelsFetchChannels, conversationsFetchConversations, contactsFetchContacts, messagesSendAMessage]
---

# Send a WhatsApp message with Rasayel

Use the Rasayel REST API (`https://api.rasayel.io/v1/`) to send a WhatsApp message.

## Auth
Every request carries `Authorization: Basic {token}` where `{token}` is the
"Basic Auth Value" of a **Read/Write** scoped Rasayel API key. Optionally set
`X_On_Behalf_Of: {user_id}` to act as a specific user.

## Steps
1. **Pick the channel.** `channelsFetchChannels` (`GET /channels`) — list the
   connected WhatsApp Business channels and choose the `id` to send from.
2. **Locate the conversation/contact.** `conversationsFetchConversations`
   (`GET /conversations?phone={e164}`) to find an existing conversation, or
   `contactsFetchContacts` (`GET /contacts`) to resolve the recipient.
3. **Send the message.** `messagesSendAMessage` (`POST /messages`) with the
   channel, recipient, and message body (free-form text is only allowed inside
   the 24-hour customer service window; otherwise send an approved template).

## Rules
- Respect rate limits: leaky bucket, 100-token capacity, 20 req/min refill.
  Watch the `X_Rasayel_Api_Call_Capacity` header and honor `Retry-After` on 429.
- Outside the 24-hour window you must use an approved message **template**
  (see the "Manage WhatsApp templates" skill).
- No idempotency key exists — do not blindly retry `POST /messages` on a
  network timeout; re-fetch the conversation to confirm before resending.
