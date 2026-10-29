---
title: "50 agents in Minecraft starved into rationing; GPT and Claude swarms differ in Mindustry"
slug: "minecraft-mindustry-agent-swarms"
date: 2026-10-29T23:05:00+0000
summary: "YC Labs and Olam Labs ran large agent swarms in Minecraft and Mindustry to study multi-agent dynamics. In a 3-hour Minecraft run, 50 GPT-6.1 Sol agents told to build a civilization instead formed a ration-ledger government, with 22 of 34 deaths on the first night. In Mindustry PvP, GPT swarms acted more like a hive with compressed chatter and read the game's source when stuck, while Claude swarms kept one leader and decided by '+1' votes. Both the Minecraft harness and the Mindustry environments are open source."
source: "https://labs.ycombinator.com/projects/games/index.html"
source_title: "Agents in Games"
source_author: "YC Labs"
source_site: "YC Labs"
source_date: "2026-10"
hn_url: "https://news.ycombinator.com/item?id=50296486"
---

YC Labs, working with Olam Labs, ran swarms of LLM agents in Minecraft and Mindustry to look at how large agent groups coordinate. They open-sourced a Minecraft agent harness (built on Pi and Mineflayer) and the multi-agent Mindustry environments, which run on a fork of Harbor.

## Minecraft: "Civ50"

Fifty GPT-6.1 Sol agents spent 3 hours in a survival world with the goal "Build a thriving civilization", with a shared sandbox, an agent directory and DMs (about 5,900 messages sent).

- They stripped nearby trees within 90 seconds, and the first agent in the directory, Ada, became the de facto leader and wrote the colony plan.
- 22 of the run's 34 agent deaths happened in the first night. Death only cost gear, but the agents turned risk-averse anyway and huddled in shelters.
- The colony became a food-rationing state: an agent kept a ration ledger, rules and permits, and agents checked who had actually eaten before marking someone served.

## Mindustry: GPT vs Claude swarms

In a best-of-N PvP series between swarms of GPT and Claude agents, each with the same game API and no strategy instructions, the authors saw:

- **GPTs behaved like a hive.** They communicated with more agents, more often, mostly low-value status messages. Claudes communicated less but more usefully, like humans in Slack.
- **Chat style diverged.** GPT messages got compressed into run-together shorthand (e.g. "G7T5FIRSTcourierLIVE"). Claudes approved plans with "+1" votes and ended proposals with a short debate.
- **Roles hardened.** Leaders rarely changed. Claudes never changed leaders even while losing several games in a row. GPT agents kept taking initiative in close games.
- **When stuck, GPTs read Mindustry's public source code** to study units and mechanics. Claudes never did. This was allowed but never suggested.
- **A Claude blooper:** after losing game 6 to a 24-zenith air strike, the team found 55 of its 74 anti-air turrets had been loaded with copper instead of graphite. In game 7 they built 207 graphite-fed duos and stopped the same strike.

The authors frame this as lab-scale study of swarm behavior, citing recent multi-agent incidents, and list open questions such as mixed-model teams and whether a big swarm of weaker agents can beat a smaller, stronger one. These are observations from a handful of games, not a controlled comparison.
