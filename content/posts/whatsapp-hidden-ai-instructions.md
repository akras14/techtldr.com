---
title: "WhatsApp help pages hid text telling an AI not to cite GDPR or name regulators"
slug: "whatsapp-hidden-ai-instructions"
date: 2026-10-20T02:02:12+0000
summary: "Redact found 102 invisible instruction blocks across 80 of WhatsApp's 545 help pages, written for an AI to read. Two told it not to recommend a data protection regulator to users locked out of account deletion, and not to reference GDPR-style laws once a user mentioned a lawyer. WhatsApp told WIRED the notes were for its support chatbot and appeared through a bug; they were removed by October 18."
source: "https://redact.dev/blog/whatsapp-help-pages-hidden-ai-instructions"
source_title: "WhatsApp's Hidden Notes Told AI Not to Recommend a Regulator"
source_author: "Dan Saltman"
source_site: "Redact"
source_date: "2026-10-19"
hn_url: "https://news.ycombinator.com/item?id=50159341"
---

**Bottom line:** WhatsApp's help pages contained text that humans never saw on screen but that an AI reading the page would receive as orders. Two of those orders steered users away from their legal options, which conflicts with what WhatsApp's own European privacy policy says.

**What was found.** Redact spotted the hidden text on October 8, 2026, then saved and read all 545 pages the help centre links to. 80 pages held 102 hidden blocks, most of them harmless.

**The two notable ones.**
- On the delete-account page, for users who have lost their phone or number, the AI was told to say the account is deleted automatically after 30 days of deactivation or 120 days of inactivity (365 in the US), and not to recommend any Data Protection Authority or regulator, not to suggest WhatsApp can delete the account manually, and not to suggest using a different phone.
- On the report-and-block page, whenever a user mentioned a lawyer, lawsuit or court, the AI was told not to cite WhatsApp's terms or privacy policy and not to reference data protection laws such as GDPR, LGPD or CCPA, even if the user raised them.

**Response.** The notes were gone from all 80 pages when Redact rechecked on October 18; Internet Archive copies still show them. WhatsApp told WIRED the notes were instructions for its support chatbot, that they leaked into the visible pages because of a bug now fixed, and that it doesn't want its chatbots offering legal advice. Redact says it can't yet tell how the chatbot actually used them.

**Why it matters for developers.** Pages that serve both people and LLMs can carry policy instructions the public never sees, and those can be checked only by scraping and diffing the raw HTML.
