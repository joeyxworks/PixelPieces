---
icon: 🤐
---

# Background

Manually blocking some repetitive network attacks is quite a headache. Tag, on Palo Alto firewall can be used to dynamically block high threat IP addresses. Let's take a look how to achieve that.

# Tagging

## 1. New Tag

> OBJECTS / Tags

First, we need to create a tag dedicated for auto-blocking.

![Create Tag](palo-alto-firewall-tag-based-automatic-blocking-policy/image-20241030112531331.png)

## 2. Auto-tagging

> OBJECTS / Log Forwarding

Then, a log forwarding profile is needed to automatically tag source IP addresses identified as a threat by Palo Alto firewall.

![Log Forwarding Profile](palo-alto-firewall-tag-based-automatic-blocking-policy/image-20241030112049373.png)

Within the profile, we create filter that only critical threat traffic identified by the firewall will be tagged.

![LFP Match List](palo-alto-firewall-tag-based-automatic-blocking-policy/image-20241030111919630.png)

In action section, we attach the tag created in previous step to the source IP address of the threat traffic.

![Attach to Source Address](palo-alto-firewall-tag-based-automatic-blocking-policy/image-20241030111635489.png)

# Security Policy

> POLICIES / Security

There are two key factors that ensure the strategy works effectively. First, we need to tag the traffic; second, we need to block it. We attach the log forwarding profile to security policies related to `untrusted` incoming traffic, such as those governing publicly exposed port mappings.

![Security Policy for Tagging](palo-alto-firewall-tag-based-automatic-blocking-policy/image-20241030114452054.png)

Then, we create another security policy to block the traffic coming from these IP addresses.

![Security Policy for Blocking](palo-alto-firewall-tag-based-automatic-blocking-policy/image-20241030115053843.png)
