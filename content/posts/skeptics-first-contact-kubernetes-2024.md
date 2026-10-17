---
title: "Kubernetes explained as control loops and services, by a skeptic (2024)"
slug: "skeptics-first-contact-kubernetes-2024"
date: 2026-10-17T20:03:00+0000
summary: "A longtime infrastructure engineer who had avoided Kubernetes argues most of its value comes from two ideas: control loops (controllers that drive observed state toward desired state) and Services (a virtual IP plus kube-proxy rules on every node). He ends up liking both, and complaining about Helm's text templating and stringly-typed configs. He hadn't operated a real cluster when he wrote it."
source: "https://blog.davidv.dev/posts/first-contact-with-k8s/"
source_title: "A skeptic's first contact with Kubernetes"
source_author: "David Ventura"
source_site: "davidv.dev"
source_date: "2024-07-13"
hn_url: "https://news.ycombinator.com/item?id=50127572"
---

David Ventura, who spent years in sysadmin and infrastructure work without touching Kubernetes, read up on it to replace a vague bias with facts. His conclusion: most of the value comes from **control loops** and **Services**, and he likes both. He says up front he never ran a cluster, so this is a design critique, not operating experience.

**Control loops.** Every controller watches a sensor and adjusts a control element toward a desired state, for example queue depth driving worker count, or latency driving pod count. The sensor needn't be directly affected by the control element.

**Services.** A Service gets a virtual IP and DNS record. kube-proxy on every node watches Service objects and rewrites netfilter rules to forward that IP to backing pods, so every node is a load balancer with no single point of failure and forwarding capacity that grows with the cluster. The tradeoffs he notes: every node holds every rule (iptables evaluates linearly), and traffic may be forwarded twice after a pod moves, but clients that don't re-resolve DNS keep working.

**Workloads and storage.** Bare Pods aren't rescheduled; a ReplicaSet keeps the count, a Deployment adds rolling image updates via new and old ReplicaSets. PersistentVolumes hide the backing driver, so a pod can't rely on features like atomic renames. He questions why StatefulSets update in descending order.

**Complaints.** Autoscaling on custom signals like queue depth feels bolted on; storage and networking are out of tree (CSI/CNI) while cloud providers get built-in support; and Helm's YAML-by-text-template with manual indentation and stringly-typed values (a KEDA RabbitMQ scaler whose `value` type depends on `mode`) strikes him as the real pain.
