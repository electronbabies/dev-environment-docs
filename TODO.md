# TODO

## AI
 - Start using ChatGPT in a CLI
## Browser 
- Enable Brave Sync for bookmarks and browser settings.
- Decide whether to sync bookmarks with phone.
- Replace LastPass with Bitwarden.
- Find a reliable way to suppress Brave's restore-session prompt.
- Audit installed browser extensions.

## Nextcloud 

- Resolve Galaxy S25 storage permission issues.
- Implement a true backup strategy before relying solely on Nextcloud.
- Consider migrating completely away from Google Photos.
- Consider retiring OneDrive after backup strategy is complete.
- Document Docker backup and restore procedures.
- Add monitoring and health checks.

## Hyprland 

- Split `hyprland.conf` into multiple configuration files.
- Move configuration into the dotfiles repository.
- Document monitor arrangement with screenshots.
- Add screenshots of the desktop layout.

## Project Organization

- Keep `~/code` as the canonical development-project root.
- Keep intentional non-code Git repositories in semantically appropriate locations and register them in `repos` when they need to participate in the checkpoint.
- Run `repos` as the central Git checkpoint; use `repos --fetch` when fresh remote state matters.
