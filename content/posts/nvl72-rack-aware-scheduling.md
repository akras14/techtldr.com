---
title: "Rack-aware scheduling gave a 14.4% training speedup on NVL72 clusters"
slug: "nvl72-rack-aware-scheduling"
date: 2026-10-29T17:02:52+0000
summary: "SkyPilot argues that NVIDIA's NVL72 racks only pay off if the scheduler keeps multi-node jobs inside one rack, where all 72 GPUs share NVLink. Reserving two spare nodes per rack (16-node slices) gave 27% more usable capacity than allocating whole 18-node racks at a roughly 2% node failure rate, and rack-aware placement sped up an NVIDIA GR00T pre-training run by 14.4%."
source: "https://skypilot.ai/blog/grace-blackwell-needs-a-new-scheduler"
source_title: "Grace Blackwell needs a new scheduler: 27% more usable GPUs from the same racks"
source_author: "Rohan Sonecha"
source_site: "SkyPilot"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50290912"
---

**Bottom line:** NVL72 puts 72 GPUs on one NVLink fabric, but a job spread across racks falls back to slower InfiniBand. SkyPilot says a topology-aware scheduler is what turns the hardware into real gains. This is a vendor post promoting its own platform.

## Why placement matters

- Tensor and expert parallelism put communication on the critical path. Moving tensor parallelism from NVLink to InfiniBand cost Hugging Face a 43% throughput drop.
- On 8-GPU servers, an expert-parallel group can span at most 8 GPUs before leaving NVLink. In an NVL72 rack, groups of up to 64 GPUs can stay on NVLink. NVIDIA reports up to 4.4x higher per-GPU DeepSeek-R1 serving throughput with 32-way expert parallelism on GB200 NVL72 versus 8-GPU B200 servers.

## Leave spare nodes

- The rack becomes the failure unit. Using Meta's figure of about 490 days between interruptions per 4-GPU node, a rack is interrupted roughly every 27 days, so a 128-rack cluster loses a full-rack job slot about every 5 hours.
- At the roughly 2% unavailability SkyPilot sees on production NVL72 clusters, NVIDIA's model gives 16-node slices (two spares per rack) 27% more usable capacity than 18-node slices, and 4% more than 9-node slices.

## What SkyPilot does

- **Topology-aware scheduling:** keeps each slice on a single rack. NVIDIA's GR00T team measured a 14.4% throughput gain (1.59 s to 1.39 s per iteration) on 512 GB300 GPUs. SkyPilot extrapolates that to about $400K saved per training day on a 10,000-GPU cluster.
- **In-place recovery:** swaps a failed node for a healthy spare in the same rack, rescheduling the slice elsewhere only if none remain.
- **Auto defrag:** preempts the least disruptive restartable workloads (such as checkpointed managed jobs) to free a whole rack for a blocked large job.
