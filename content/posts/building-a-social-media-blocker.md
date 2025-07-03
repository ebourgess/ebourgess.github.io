---
layout: post
title:  "[Bash] Building a Simple Social Media Blocker with Bash"
date:   2025-07-03
authors: ["Elias Bourgess"]
slug: "building-a-social-media-blocker"
categories: ["Bash"]
draft: false
---

In our hyper-connected world, social media platforms are designed to capture and hold our attention. While these platforms have their benefits, they can also become significant sources of distraction, especially when you're trying to focus on work, study, or simply enjoy some offline time.

Today, I'll walk you through a simple yet effective solution I built: a bash script that blocks distracting websites at the system level by modifying your computer's hosts file.

## The Problem

We've all been there—you sit down to work on an important project, but somehow find yourself scrolling through Reddit, Instagram, or Twitter (X) for hours. Browser extensions and app blockers can help, but they're often easy to disable when temptation strikes. What if we could block these sites at a deeper level?

## The Solution: Hosts File Blocking

The solution lies in your system's `/etc/hosts` file. This file acts as a local DNS lookup table, allowing you to override where domain names point. By redirecting social media domains to `127.0.0.1` (localhost), we can effectively make them unreachable from any browser or application on your system.

## How It Works

The script I created does three main things:

1. **Block Mode**: Adds entries to `/etc/hosts` that redirect specified domains to localhost
2. **Unblock Mode**: Removes these entries to restore normal access
3. **Safety Features**: Creates backups and uses markers for clean removal

Let's break down the key components:

### The Core Script

```bash
#!/bin/bash

HOSTS_FILE="/etc/hosts"
BLOCKLIST="$HOME/.blocklist"
MARKER="# SOCIAL_MEDIA_BLOCK"
```

We define three important variables:
- `HOSTS_FILE`: Points to the system hosts file
- `BLOCKLIST`: A user-customizable list of domains to block
- `MARKER`: Comments that help us identify our entries for clean removal

### Blocking Function

```bash
block() {
    sudo cp "$HOSTS_FILE" "$HOSTS_FILE.bak"
    echo "$MARKER START" | sudo tee -a "$HOSTS_FILE" > /dev/null
    while read -r domain; do
        echo "127.0.0.1 $domain" | sudo tee -a "$HOSTS_FILE" > /dev/null
    done < "$BLOCKLIST"
    echo "$MARKER END" | sudo tee -a "$HOSTS_FILE" > /dev/null
    echo "[✓] Sites blocked."
}
```

This function:
1. Creates a backup of the hosts file
2. Adds a start marker
3. Reads each domain from the blocklist and redirects it to localhost
4. Adds an end marker
5. Confirms the operation

### Unblocking Function

```bash
unblock() {
    sudo cp "$HOSTS_FILE" "$HOSTS_FILE.bak"
    sudo sed -i.bak "/$MARKER START/,/$MARKER END/d" "$HOSTS_FILE"
    echo "[✓] Sites unblocked."
}
```

The unblock function uses `sed` to remove everything between our markers, cleanly restoring the hosts file to its original state.

## Key Features

### 1. **System-Wide Blocking**
Unlike browser extensions, this approach blocks sites across all applications—browsers, desktop apps, and even command-line tools.

### 2. **Safety First**
The script always creates a backup before making changes, so you can manually restore if something goes wrong.

### 3. **Clean Removal**
Using markers ensures that unblocking removes only our additions, leaving the rest of your hosts file intact.

### 4. **Customizable Blocklist**
The `~/.blocklist` file can be easily edited to add or remove domains based on your needs.

## Setup and Usage

Setting up the social blocker is straightforward:

1. **Copy the example blocklist**:
   ```bash
   cp blocklist.example ~/.blocklist
   ```

2. **Customize your blocklist**:
   ```bash
   vi ~/.blocklist
   ```

3. **Make the script executable**:
   ```bash
   chmod +x social_block.sh
   ```

4. **Block sites when you need focus**:
   ```bash
   ./social_block.sh block
   ```

5. **Unblock when you're done**:
   ```bash
   ./social_block.sh unblock
   ```

## The Default Blocklist

The example configuration blocks some of the most common distracting sites:

- Reddit (reddit.com, www.reddit.com)
- Instagram (instagram.com, www.instagram.com)
- X/Twitter (x.com, www.x.com)
- Facebook (facebook.com, www.facebook.com)

You can easily customize this list by editing `~/.blocklist` to match your specific distraction triggers.

## Why This Approach Works

### Friction by Design
The key insight is that most of our social media browsing is habitual and unconscious. By adding friction—making these sites unreachable—we break the automatic behavior and create space for conscious decision-making.

### System-Level Control
Unlike browser-based solutions, this method works across all applications. Even if you try to access these sites through a different browser or app, they'll still be blocked.

### Simple Toggle
With just two commands, you can easily switch between "focus mode" and normal browsing, making it practical for daily use.

## Potential Improvements

While this script works well as-is, here are some ideas for enhancement:

1. **Time-based blocking**: Automatically enable blocking during work hours
2. **Whitelist mode**: Block everything except specified sites
3. **Logging**: Track how often you attempt to access blocked sites
4. **GUI interface**: Create a simple graphical interface for non-technical users

## Considerations and Limitations

### Administrative Access Required
The script requires `sudo` access since it modifies system files. This is both a security feature (it can't be easily bypassed) and a potential inconvenience.

### DNS Caching
Some systems cache DNS lookups, so you might need to flush your DNS cache or restart your browser for changes to take effect immediately.

### VPN and Alternative DNS
This method won't work if you're using a VPN that routes DNS requests externally, or if you've configured your system to use alternative DNS servers that bypass the hosts file.

## Conclusion

This simple bash script demonstrates how a few lines of code can create an effective digital wellness tool. By leveraging the hosts file, we create system-wide blocking that's harder to bypass than browser-based solutions, while still maintaining the flexibility to unblock sites when needed.

The beauty of this approach lies in its simplicity. There's no complex software to install, no accounts to create, and no ongoing maintenance required. It's just a straightforward script that puts you back in control of your digital attention.

Whether you're a developer looking to minimize distractions during coding sessions, a student preparing for exams, or anyone wanting to be more intentional about their online time, this social media blocker provides a simple yet effective solution.

The complete code is available on [GitHub](https://github.com/ebourgess/social-blocker), and I encourage you to customize it for your specific needs. Sometimes the most effective tools are the simplest ones.

---

*Have you tried similar approaches to managing digital distractions? I'd love to hear about your experiences and any improvements you might suggest on my [email](mailto:elias@ebourgess.dev).*
