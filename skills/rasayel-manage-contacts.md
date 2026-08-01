---
name: Manage Rasayel contacts, tags, and properties
description: Create and update contacts, then organize them with tags and custom properties via the Rasayel REST API.
api: openapi/rasayel-rest-openapi.yml
operations: [contactsFetchContacts, contactsCreateContact, contactsUpdateContact, contactsTagContact, propertiesFetchProperties, contactsAttachPropertiesToContact]
---

# Manage Rasayel contacts, tags, and properties

Use the Rasayel REST API (`https://api.rasayel.io/v1/`) to maintain your contact
records.

## Auth
Send `Authorization: Basic {token}` with a **Read/Write** API key on every
request.

## Steps
1. **Look up or list contacts.** `contactsFetchContacts` (`GET /contacts`) —
   paginate with `page` and `per_page`; read the `pagination` object to walk
   pages.
2. **Create a contact.** `contactsCreateContact` (`POST /contacts`) with name /
   phone / email fields. The created `Contact` object is returned directly.
3. **Update a contact.** `contactsUpdateContact` (`PATCH /contacts/{id}`).
4. **Tag it.** `contactsTagContact` (`POST /contacts/{id}/tag`); remove with
   `DELETE /contacts/{id}/tag`.
5. **Attach custom properties.** First `propertiesFetchProperties`
   (`GET /properties`) to discover property definitions, then
   `contactsAttachPropertiesToContact` (`POST /contacts/{id}/properties`).

## Rules
- List endpoints are paginated (`{ data, pagination }`); single-resource reads
  return the object directly.
- Honor rate limits (leaky bucket, 20 req/min refill) — back off on `429` using
  `Retry-After`.
