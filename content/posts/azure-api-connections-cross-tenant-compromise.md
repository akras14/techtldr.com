---
title: "Azure API Connections: five cross-tenant takeovers, $200,000 in bounties"
slug: "azure-api-connections-cross-tenant-compromise"
date: 2026-10-30T13:05:00+0000
summary: "A researcher at Binary Security found five critical elevation-of-privilege bugs in Azure Logic Apps API Connections, each letting an attacker use any other customer's connection worldwide. That meant access to the victim's connected backend, such as Key Vaults, Azure SQL databases, Jira or Salesforce. The bugs paid about $200,000 in total, and Microsoft has fixed all of them."
source: "https://www.binarysecurity.no/posts/2026/10/one-root-case"
source_title: "Azure's Weakest Link - Five Full Cross-Tenant Compromises"
source_author: "Haakon Wik Gulbrandsrud"
source_site: "Binary Security"
source_date: "2026-10-02"
hn_url: "https://news.ycombinator.com/item?id=50301854"
---
The bottom line: Azure's API Connections design routes every call through a shared API Management (APIM) instance that holds the stored credentials for all customers' connections. The only thing keeping tenants apart was input validation, and that validation kept failing. The author reports five critical bugs in this one system, fixed by Microsoft after reporting, worth about $200,000.

## How it works

A logic app calls the shared APIM instance. APIM checks the request against the connector's OpenAPI definition, swaps the caller's token for the configured backend credential, and forwards the call. If an attacker can make APIM act on someone else's connection, it uses the victim's stored token against the victim's backend. The author notes that connections are rarely scoped to minimal privileges, and OAuth connections carry the creating user's own token.

## The bugs

- **Path traversal in `DynamicInvoke`.** Calling the ARM REST endpoint with `../` segments in the request path let the author read a secret from another tenant's Key Vault. Microsoft's first fix was a path blocklist.
- **Undocumented siblings.** The same code held three more undocumented functions, such as `DynamicList`, reachable by renaming the endpoint. They had the same flaw and gave arbitrary SQL execution on a victim's database. Microsoft's fix was to block the endpoints rather than fix the underlying issue.
- **`host.api.runtimeUrl` in Consumption Logic Apps.** Consumption apps create Version 1 connections with no per-app identity authorization, so any such app has implicit access to every V1 connection. The runtime validated which connection you referenced, but accepted an alternative runtime URL for any host ending in `azure-apihub.net`, and attached the auth header.
- **Validation before normalization.** Microsoft's fix required the runtime URL to match the expected one, but the check ran before path normalization. Traversal segments passed the check, and APIM then resolved them to the victim's connection.
- **Regression.** A later fix for that traversal stopped validating the original runtime URL path, which reopened the same attack.

## Takeaway

The author's argument is that blocklists and validation layered over a shared credential-holding proxy are fragile, and that the architecture lacks real per-tenant isolation. He reports that Microsoft fixed everything within a reasonable time, with some disagreements with MSRC along the way.
