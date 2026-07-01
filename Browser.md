# Browser
## Overview

My primary browser is **Brave**.

The browser setup is intentionally simple. Brave is used as a fast, Chromium-compatible, privacy-oriented browser with only a small number of extensions installed. Unlike other parts of the system, the browser is not heavily customized and does not currently integrate directly with Nextcloud or Obsidian.

The goal is for the browser to stay boring, reliable, and easy to recreate.

## Browser Choice

### Brave

Brave is the default browser on the Fedora desktop.

Reasons for using Brave:

* Chromium compatibility
* Good performance
* Built-in privacy features
* Works well with common web apps
* Supports the extensions I care about
* Does not require Google account sync

Brave is used as the general-purpose browser for daily browsing, web apps, research, documentation, and development-related work.

## Search Engine

The current default search engine is **Brave Search**.

This fits the general privacy-conscious direction of the rest of the setup while requiring no extra maintenance.

## Browser Sync

Brave Sync is **not currently enabled**.

This means bookmarks, browser settings, and extensions are not automatically synchronized between devices.

Current devices are not tightly synced at the browser level:

* Fedora desktop does not sync Brave data elsewhere
* Phone bookmarks are not synced with desktop bookmarks
* Previous Windows/Linux browser switching caused bookmark friction
* Current setup no longer regularly switches between Windows and Linux

### Current Philosophy

Browser data is mostly local right now.

This is acceptable for daily use, but bookmarks are important enough that syncing them is probably worth revisiting.

## Bookmarks

Bookmarks are organized using **nested folders**.

This works well enough locally, but the lack of sync has been annoying in the past. When moving between systems, bookmarks created on one machine were not available on the other.

Bookmarks are one of the few browser features that probably should be synchronized in the future.

## Password Manager

The current password manager is **LastPass**.

This is functional and already part of the current workflow, but it is a candidate for replacement in the future.

A likely future replacement is **Bitwarden**, since it fits well with the rest of the system: cross-platform, browser-friendly, phone-friendly, and less tied to a single desktop setup.

Password manager migration is intentionally deferred for now.

## Extensions

The Brave extension setup is minimal.

Installed extensions:

| Extension              | Purpose                                          |
| ---------------------- | ------------------------------------------------ |
| Vimium                 | Keyboard-first browser navigation                |
| LastPass               | Password management                              |
| Language Reactor       | Japanese language learning while watching videos |
| Return YouTube Dislike | Restores estimated dislike counts on YouTube     |

### Vimium

Vimium is the most important browser extension in this setup.

It provides Vim-style keyboard navigation in the browser, which fits naturally with the rest of the keyboard-driven desktop environment.

Vimium supports the same general philosophy as Hyprland, Neovim, tmux, and the Moonlander: keep hands on the keyboard and reduce unnecessary mouse usage.

### Language Reactor

Language Reactor is used for Japanese learning.

This is mainly useful when watching video content and studying Japanese through subtitles, translations, and playback assistance.

### Return YouTube Dislike

Return YouTube Dislike restores estimated dislike counts on YouTube.

This is a small quality-of-life extension.

## VPN

ExpressVPN is installed, but it is not integrated into Brave.

It runs as its own separate application/window rather than as a Brave extension.

VPN configuration should be documented separately if it becomes part of a broader network or security page.

## Downloads

Browser downloads use the default location:

```text
~/Downloads
```

This is intentional enough for now.

Downloads are treated as temporary files. Anything important should eventually be moved somewhere more permanent, such as the appropriate Nextcloud-synced folder.

## Session Restore

Brave does **not** automatically restore the previous session.

However, the restore prompt that appears after Brave does not shut down cleanly is annoying.

Previous attempts to suppress this popup were not successful.

This should remain a future improvement rather than something the current setup depends on.

## Integration With the Rest of the System

The browser is mostly independent.

It does not currently sync with:

* Nextcloud
* Obsidian
* Phone browser bookmarks
* Another desktop browser profile

This is different from other parts of the system, where Nextcloud and Obsidian are central pieces.

For the browser, the current approach is simpler:

* Brave handles browsing
* LastPass handles passwords
* Downloads go to `~/Downloads`
* Permanent files are moved elsewhere manually
* Bookmarks are currently local

## Maintenance

Browser maintenance is minimal.

Occasional maintenance tasks:

* Review installed extensions
* Remove unused extensions
* Keep Brave updated through the system package manager
* Review bookmark organization
* Consider enabling Brave Sync
* Revisit password manager choice

## Troubleshooting

### Restore Previous Session Prompt Appears

Sometimes Brave shows a prompt asking to restore the previous session after an unclean shutdown.

Current status:

* This is annoying
* It has been investigated before
* No reliable fix has been found yet
* It is not currently blocking normal use

### Missing Bookmarks on Another Device

Bookmarks are not currently synced.

If a bookmark is missing on another device, it is probably because Brave Sync is not enabled.

## Future Improvements

See: [TODO → Browser](TODO.md#browser)