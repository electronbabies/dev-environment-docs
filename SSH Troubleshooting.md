# SSH Troubleshooting

Common SSH problems, what they mean, and how to fix them.

---

## Remote Host Identification Has Changed

### Error

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!

Someone could be eavesdropping on you right now (man-in-the-middle attack)!

It is also possible that a host key has just been changed.

Host key for example.com has changed and you have requested strict checking.

Host key verification failed.
```

### What It Means

SSH remembers the identity of servers you have previously connected to.

The server's public host key is stored locally in:

```text
~/.ssh/known_hosts
```

When connecting again, SSH compares the server's current host key against the one it previously saved.

If they don't match, SSH refuses the connection because this **could indicate a man-in-the-middle attack**.

However, this warning is also expected when a server has legitimately been replaced or its SSH host keys have been regenerated.

### Common Legitimate Causes

- A server or VM was rebuilt.
- A DigitalOcean Droplet was deleted and recreated.
- A hostname was pointed to a new server.
- SSH host keys were intentionally regenerated.
- An IP address was reassigned to another machine.

### Fix

**Only do this if you know why the host key changed.**

Remove the old host key:

```bash
ssh-keygen -R example.com
```

For an IP address:

```bash
ssh-keygen -R 192.0.2.10
```

Then reconnect:

```bash
ssh user@example.com
```

SSH will treat the server as a new host and display its fingerprint:

```text
The authenticity of host 'example.com' can't be established.
ED25519 key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Verify the fingerprint when appropriate, then enter:

```text
yes
```

The new host key will be written to:

```text
~/.ssh/known_hosts
```

### Do Not Disable Host-Key Checking

Do **not** solve this by globally disabling SSH host-key verification.

Avoid configurations such as:

```text
StrictHostKeyChecking no
```

The warning exists for an important reason. If the server has **not** intentionally changed, investigate before removing the existing key.

---

## Permission Denied (publickey)

### Error

```text
Permission denied (publickey).
```

### What It Means

The SSH server requires public-key authentication, but it did not accept any of the keys offered by the client.

Common causes include:

- Your public key has not been installed on the server.
- SSH is trying the wrong private key.
- File permissions on the server are incorrect.
- The username is incorrect.
- An SSH agent is not running or does not contain the expected key.

### Check Existing Local Keys

```bash
ls -la ~/.ssh
```

A typical Ed25519 key pair looks like:

```text
id_ed25519
id_ed25519.pub
```

The file without `.pub` is the **private key**.

Never share it.

The `.pub` file is the **public key** and can safely be installed on servers.

Display it with:

```bash
cat ~/.ssh/id_ed25519.pub
```

### Install the Public Key on the Server

The public key must appear in the remote user's:

```text
~/.ssh/authorized_keys
```

Create the SSH directory if necessary:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Add the public key:

```bash
nano ~/.ssh/authorized_keys
```

Each public key should occupy a single line.

Then set the correct permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

Try connecting again:

```bash
ssh user@example.com
```

### Explicitly Specify a Key

If SSH is selecting the wrong identity:

```bash
ssh -i ~/.ssh/id_ed25519 user@example.com
```

For more information about what SSH is attempting, use verbose mode:

```bash
ssh -v user@example.com
```

For substantially more detail:

```bash
ssh -vvv user@example.com
```

---

## Could Not Open a Connection to Your Authentication Agent

### Error

```text
Could not open a connection to your authentication agent.
```

This commonly appears when running:

```bash
ssh-add -l
```

### What It Means

`ssh-add` communicates with an SSH authentication agent.

The error means the current shell does not have access to a running `ssh-agent`.

This **does not necessarily mean SSH itself will fail**.

SSH can use a private key such as:

```text
~/.ssh/id_ed25519
```

directly without the key being loaded into an agent.

### Check Whether SSH Works First

Before changing anything, simply try:

```bash
ssh user@example.com
```

If authentication succeeds, there may be nothing that needs fixing.

### Start an SSH Agent Manually

If an agent is actually needed:

```bash
eval "$(ssh-agent -s)"
```

Then add the key:

```bash
ssh-add ~/.ssh/id_ed25519
```

Verify:

```bash
ssh-add -l
```

> Do not automatically add manual `ssh-agent` startup to shell configuration without first checking whether the desktop/session environment already provides an agent.

---

## Useful SSH Files

### Local Machine

```text
~/.ssh/
├── config
├── id_ed25519
├── id_ed25519.pub
├── known_hosts
└── known_hosts.old
```

#### `id_ed25519`

Private SSH key.

**Never share, upload, or commit this file.**

Recommended permissions:

```bash
chmod 600 ~/.ssh/id_ed25519
```

#### `id_ed25519.pub`

Public SSH key.

This is the key installed on remote servers.

#### `known_hosts`

Stores the identities of servers previously contacted by SSH.

This protects against connecting unknowingly to a different machine using the same hostname or IP address.

#### `config`

Optional client configuration used to create aliases and specify connection settings.

Example:

```sshconfig
Host example
    HostName example.com
    User deploy
    IdentityFile ~/.ssh/id_ed25519
```

This allows:

```bash
ssh example
```

instead of:

```bash
ssh deploy@example.com
```

---

## Server-Side SSH Files

For a given remote user:

```text
~/.ssh/
└── authorized_keys
```

Recommended permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

The `.ssh` directory and its files should also be owned by the user who is authenticating.

Check with:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

---

## Debugging an SSH Connection

When an SSH connection fails and the reason isn't obvious, verbose output is usually the best first diagnostic.

### Basic Verbose Output

```bash
ssh -v user@example.com
```

### More Verbose

```bash
ssh -vv user@example.com
```

### Maximum Verbosity

```bash
ssh -vvv user@example.com
```

Look for lines involving:

```text
Offering public key
Authentications that can continue
Server accepts key
identity file
```

These show which keys SSH discovered, which keys it attempted, and what authentication methods the server accepts.

---

## Emergency Access to a DigitalOcean Droplet

If normal SSH access is unavailable, the DigitalOcean web console can be used to regain access to the machine.

Typical recovery flow:

1. Open the Droplet in DigitalOcean.
2. Open its web console.
3. Log in to the server.
4. Inspect or repair `~/.ssh/authorized_keys`.
5. Confirm SSH directory/file permissions.
6. Test SSH again from the local machine.

The browser console should primarily be considered a **recovery mechanism**.

For normal server administration, use SSH from the local terminal.

The web console can have noticeable latency, which makes interactive terminal editors such as Vim/Neovim unpleasant to use. A simple editor such as `nano` is perfectly reasonable when performing emergency changes through the web console.

---

## Quick Reference

### Remove a stale server identity

```bash
ssh-keygen -R example.com
```

### Connect to a server

```bash
ssh user@example.com
```

### Connect with a specific key

```bash
ssh -i ~/.ssh/id_ed25519 user@example.com
```

### Show your public key

```bash
cat ~/.ssh/id_ed25519.pub
```

### List keys loaded in the SSH agent

```bash
ssh-add -l
```

### Start an SSH agent

```bash
eval "$(ssh-agent -s)"
```

### Add a key to the agent

```bash
ssh-add ~/.ssh/id_ed25519
```

### Debug a connection

```bash
ssh -vvv user@example.com
```

### Correct server-side SSH permissions

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```