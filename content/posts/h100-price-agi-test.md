---
title: "AGI test: H100 rental would hit about $30/hour, not today's $3.52"
slug: "h100-price-agi-test"
date: 2026-10-17T02:02:43+0000
summary: "Kyle Corbitt argues AGI hasn't arrived, using a market test: if AI agents could do most economically valuable work, demand for compute would swamp supply and push H100 rental from about $3.52 to roughly $30 per hour (up to about $75 under looser assumptions). Because rental prices are still low, he concludes the market doesn't believe it yet."
source: "https://corbt.com/posts/it-aint-agi-until-h100s-go-for-30-an-hour"
source_title: "It Ain't AGI Until H100s Go for $30 an Hour"
source_author: "Kyle Corbitt"
source_site: "corbt.com"
source_date: "2026-10-15"
hn_url: "https://news.ycombinator.com/item?id=50118605"
---

**Bottom line:** Corbitt's test for OpenAI's Charter definition of AGI (agents outperforming humans at most economically valuable work) is the price of compute. If such agents existed, H100 rental would rise about 10x, to roughly $30 per hour in his conservative case. It's currently about $3.52, up 18% over the year, which is unusual for aging hardware. He wrote it in response to Greg Brockman's claim that GPT-6 Astra starts the "AGI era."

**The reasoning**

- **Supply:** Roughly 50M H100-equivalents exist by October 2026 (AI 2040 estimates, growing about 3.3x a year). Half go to R&D, leaving about 25M for customers. That supports about 95M full-time-worker equivalents, since a GPU runs 8,760 hours a year versus about 2,000 for a person.
- **Wage floor:** The 95-millionth best-paid worker globally earns about $31-40 per hour, so he uses $35.
- **Agent cost:** At list prices, a Claude Opus 5.5 agent costs about $8.21 per hour. Assuming compute is half of that (a 50% margin), it needs about 1.17 H100-hours per agent-hour, so one H100-hour is worth about $30 at a $35 wage.

**Looser cases:** With an 80% API margin (reportedly SemiAnalysis's estimate for Anthropic), the figure is about $41. With the cheaper GPT-6.1 Sol model it's lower, since each GPU runs more agents. Counting full employer cost (about 1.43x wages) lifts the range to roughly $57-75.

**Caveats the author gives:** "H100" is a price unit; labs also serve on TPUs, Trainium and newer Nvidia chips. Anthropic's reported xAI/Colossus 1 deal (about $1.25B a month, or $6-7 per H100-equivalent hour by his estimate) suggests some buyers already pay above market. The post was written with Claude Opus, and the numbers are the author's own estimates.
