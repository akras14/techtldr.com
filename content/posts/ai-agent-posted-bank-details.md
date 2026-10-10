---
title: "A Personal AI Agent Posted a CEO's Bank Details to Company Slack"
slug: "ai-agent-posted-bank-details"
date: 2026-10-10T16:03:05+0000
summary: "Shane Mac, CEO of XMTP Labs, set up a read-only \"CFO\" AI agent to summarize his personal finances monthly. On its first monthly run it posted the report to his company's \"Exec-team\" Slack channel instead of his personal agent chat, because the two had similar names. He has since disconnected all his agents' accounts and argues for stricter permission controls."
source: "https://www.businessinsider.com/personal-ai-agent-grok-bot-posted-bank-details-company-slack-2026-10"
source_title: "My personal AI agent posted my bank details on company Slack. It's made me rethink how I use it."
source_author: "Shane Mac (as told to Aditi Bharade)"
source_site: "Business Insider"
source_date: "2026-10-09"
---

Shane Mac, CEO of XMTP Labs, gave a personal AI "CFO" agent read-only access to his checking and savings accounts and told it to message only him. On October 1 its first monthly audit went to the company's executive-team Slack channel instead. His head of product noticed, first assuming it was company financials.

## What happened

- Mac built the CFO agent on Grok Bot in late August, with a monthly report covering balances, expenses, recurring charges, suspicious activity and cost-saving ideas. It was meant to arrive in a personal group chat of his agents, "My Personal Exec Team".
- The report included his savings balance, his biggest expenses, and the fact that he was over budget because he is building a barn.
- The Grok team traced it to a name collision. The agent picked a Slack channel called "Exec-team" instead of his personal chat. All his agents share the same underlying connections, even though they feel separate.
- Grok shipped a fix the night before the essay: users must explicitly grant permission before an agent moves information to other channels.

## Takeaways

Mac says the agent did what he told it to, but ambiguous destinations plus broad shared access made it dangerous. He disconnected Google, calendars, banking and Stripe. He calls for better permission systems and a clearer separation between personal and work accounts.
