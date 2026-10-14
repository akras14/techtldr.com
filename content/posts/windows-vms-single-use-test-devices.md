---
title: "QA Wolf runs Windows desktop tests on single-use GCE VMs claimed like phones"
slug: "windows-vms-single-use-test-devices"
date: 2026-10-14T17:03:00+0000
summary: "QA Wolf built Windows desktop app testing by treating each Windows VM as a disposable device: a test claims a pre-booted, logged-in VM from a warm pool on Google Compute Engine, uses it once, and the VM is destroyed and replaced. Test code stays in Linux pods and drives the VM remotely, which sidesteps 2-3 minute cold boots and leftover state between runs."
source: "https://www.qawolf.com/blog/how-we-solved-windows-desktop-automation"
source_title: "E2exe: How we solved Windows desktop automation for enterprise scale"
source_author: "QA Wolf"
source_site: "QA Wolf"
hn_url: "https://news.ycombinator.com/item?id=50088643"
---
QA Wolf built Windows desktop app testing by treating each Windows VM as a disposable device: a test claims a pre-booted, logged-in VM from a warm pool on Google Compute Engine, uses it once, and the VM is destroyed and replaced. Test code stays in Linux pods and drives the VM remotely, which sidesteps 2-3 minute cold boots and leftover state between runs.

- **Problem:** Windows has no lightweight container with a desktop. Every test needs a full VM, UI Automation only works in a logged-in session, and machines keep state between runs.
- **Design:** It reuses their mobile-testing scheduler. A Kubernetes controller keeps pools of Windows VMs (custom resources for template, pool and VM), and tests request one with a `DeviceClaim`. A reconciler creates replacements, moves VMs through boot in short passes, and reclaims any claimed VM silent for 60 seconds.
- **Clean and fast:** Single-use VMs mean no cross-test contamination. Warm pools hide boot time, and pool creation falls back to other zones when one lacks capacity. Image updates happen by attrition as old VMs are used up.
- **Image:** A PowerShell script bakes Appium, the Appium Windows driver and WinAppDriver into a GCE image family. Variants include an Office image, and customers can supply setup scripts for private images.
- **Control:** The Linux runner uses Playwright over CDP for browsers and WebdriverIO/Appium for native apps, behind one `startApp()`/`launch()`/`expect` API. Apps reach the VM via short-lived signed URLs and install from .exe, .msi or .msix.
- **Visibility:** A Go desktop agent streams the screen over WebRTC (via FFmpeg) and injects input through `user32.dll`, giving live view, recordings and an element picker.
- **Security:** Traffic leaves from one IP range for customer allowlisting, private pools are workspace-scoped, Workload Identity avoids stored cloud keys, and nothing persists after a run.
