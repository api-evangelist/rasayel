---
name: Manage Rasayel WhatsApp message templates
description: List, create, update, and delete WhatsApp message templates with the Rasayel REST API.
api: openapi/rasayel-rest-openapi.yml
operations: [channelsFetchChannels, templatesFetchTemplates, templatesCreateTemplate, templatesUpdateTemplate, templatesFetchTemplate, templatesDeleteTemplate]
---

# Manage Rasayel WhatsApp message templates

WhatsApp requires an approved **template** to message a contact outside the
24-hour service window. Manage them via the Rasayel REST API
(`https://api.rasayel.io/v1/`).

## Auth
`Authorization: Basic {token}` with a **Read/Write** API key.

## Steps
1. **Pick the channel.** `channelsFetchChannels` (`GET /channels`) — templates
   are scoped to a channel.
2. **List templates.** `templatesFetchTemplates`
   (`GET /templates?channel_id={id}`) — each `Template` has `id`, `uuid`,
   `name`, `language`, and `category` (e.g. MARKETING, AUTHENTICATION).
3. **Create a template.** `templatesCreateTemplate` (`POST /templates`) with
   name, language, category, and body. Provide example values for variables.
4. **Update / fetch / delete.** `templatesUpdateTemplate`
   (`PATCH /templates/{id}`), `templatesFetchTemplate` (`GET /templates/{id}`),
   `templatesDeleteTemplate` (`DELETE /templates/{id}`).

## Rules
- Newly created templates must be approved by WhatsApp before they can be sent.
- Paginate list calls with `page` / `per_page`; honor rate limits (20 req/min
  refill, `Retry-After` on 429).
