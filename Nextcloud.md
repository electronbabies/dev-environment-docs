# Nextcloud

## Purpose

This page documents my Nextcloud environment.

Nextcloud serves as the central synchronization platform for my personal ecosystem. It keeps my desktop, Android devices, contacts, notes, and selected media synchronized while allowing me to self-host those services instead of relying entirely on commercial cloud providers.

The Docker configuration is the implementation. This page documents the architecture, workflows, maintenance procedures, and anything Future Gary is likely to forget.

---

## Architecture

### Server

- VPS Provider: DigitalOcean
- Operating System: Ubuntu 24.04.3 LTS (Noble)
- Deployment: Nextcloud All-in-One (AIO)
- Primary Administration: SSH
- Web Administration: Nextcloud AIO Interface

---

## Services

The following containers make up the Nextcloud infrastructure.

| Container                       | Purpose                                  |
| ------------------------------- | ---------------------------------------- |
| `nextcloud-aio-mastercontainer` | Manages the AIO installation and updates |
| `nextcloud-aio-apache`          | Web server and HTTPS endpoint            |
| `nextcloud-aio-nextcloud`       | Main Nextcloud application               |
| `nextcloud-aio-database`        | PostgreSQL database                      |
| `nextcloud-aio-redis`           | Redis cache and file locking             |
| `nextcloud-aio-notify-push`     | Push notifications                       |

All containers should normally report **healthy**.

---

## Storage Layout

| Host Path                                | Purpose                                          |
| ---------------------------------------- | ------------------------------------------------ |
| `/mnt/ncdata`                            | Primary Nextcloud data directory (user files)    |
| `/mnt`                                   | Additional host mount available to the container |
| Docker Volume: `nextcloud_aio_nextcloud` | Nextcloud application files (`/var/www/html`)    |

### Notes

- User files are stored in `/mnt/ncdata`.
- The Nextcloud application itself lives inside a Docker volume.
- Manual filesystem permission changes generally only affect `/mnt/ncdata`.

---

## Persistent Docker Volumes

| Volume                          | Purpose               |
| ------------------------------- | --------------------- |
| `nextcloud_aio_mastercontainer` | AIO configuration     |
| `nextcloud_aio_nextcloud`       | Nextcloud application |
| `nextcloud_aio_database`        | PostgreSQL database   |
| `nextcloud_aio_database_dump`   | Database backups      |
| `nextcloud_aio_redis`           | Redis data            |
| `nextcloud_aio_apache`          | Apache configuration  |

---

## Clients

### Fedora Desktop

**Purpose**

Primary workstation.

**Responsibilities**

- Full Nextcloud synchronization.
- Primary location for organizing files.
- Primary management interface for personal documents.

---

### Galaxy S25

**Purpose**

Primary daily phone.

**Current Status**

- Nextcloud client is currently **not working** due to Android storage permission issues.
- Future troubleshooting required.
- FolderSync is currently used for synchronization where appropriate.

---

### Pixel

**Purpose**

Android development and secondary Android device.

**Responsibilities**

- Synchronizes Obsidian vault.
- Automatically uploads images.
- Automatically uploads videos.
- Automatically uploads Signal downloads.
- Used for Android application testing.

---

## Authentication

- TOTP enabled.
- Authenticator application: **Aegis**.

Recovery codes should be stored securely outside of the phone.

---

## Synchronization Philosophy

Nextcloud is primarily used as a synchronization platform rather than the sole source of truth.

Examples include:

- Obsidian vault synchronization.
- Signal download synchronization.
- Contact synchronization.
- Cross-device file access.
- Photo and video synchronization.

Large historical photo collections currently remain backed up elsewhere.

---

## Maintenance

### Updating Nextcloud

1. Temporarily allow TCP port `8080` from my current public IP in the DigitalOcean firewall.
2. Open the Nextcloud AIO interface.
3. Update the **AIO mastercontainer** if prompted.
4. Wait while the containers stop and restart.
5. Wait until every container reports **healthy**.
6. Verify the Nextcloud web interface.
7. Perform any additional Nextcloud application updates if available.
8. Remove the temporary firewall rule.

### Notes

- Container startup can take several minutes.
- Do not interrupt startup while health checks are running.
- The mastercontainer is updated separately from the Nextcloud application.

---

## Troubleshooting

### Upload Failures

Things to check:

- Linux filesystem permissions.
- Ownership of `/mnt/ncdata`.
- Available disk space.
- Container health.

---

### Permission Problems

Files should normally be owned by:

```text
www-data:www-data
```

Most upload problems are Linux permission problems rather than Docker problems.

---

### Administration Interface Unreachable

The AIO management interface is separate from the normal Nextcloud web interface.

If the AIO interface cannot be reached:

1. Verify Docker containers are healthy.
2. Verify the DigitalOcean firewall allows temporary access to TCP port `8080`.
3. Verify the Ubuntu firewall.
4. Verify the AIO mastercontainer is running.

---

## Backup Philosophy

Nextcloud is an important synchronization platform, but it is **not currently the only copy** of my data.

Current redundancy includes:

- Documents synchronized to Fedora Desktop.
- Photos and videos still backed up to Google Photos.
- Historical OneDrive account retained.
- Contacts synchronized through Nextcloud.

Because of this redundancy, losing the VPS would be inconvenient but would not currently result in significant data loss.

---

## Things Future Gary Will Forget

- User files live in `/mnt/ncdata`.
- The AIO interface is normally **not** publicly accessible.
- Temporarily open TCP port `8080` from my current IP when performing maintenance.
- Update the AIO mastercontainer before updating the Nextcloud application.
- Wait for all containers to become **healthy** before assuming startup is complete.
- Most upload failures are Linux permission issues, not Docker issues.
- `Pictures` originated from my historical OneDrive archive.

---

See: [TODO → Nextcloud](TODO.md#nextcloud)

---

## Related

- [Fedora Desktop](Fedora%20Desktop.md)