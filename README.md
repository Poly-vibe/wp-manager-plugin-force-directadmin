# ZInstaller for DirectAdmin

ZInstaller is a self-contained DirectAdmin plugin that provides WordPress management workflows directly inside the DirectAdmin panel. It is designed for hosting providers and server administrators who want a lightweight WordPress installer/clone/uninstaller experience without requiring a separate external control panel.

## Features

- Install WordPress for DirectAdmin users and domains.
- Clone an existing WordPress installation to another domain or directory.
- Remove WordPress installations from the DirectAdmin user interface.
- Detect WordPress installations from user home directories.
- Run long tasks in the background with a loading/status overlay.
- Support DirectAdmin admin, reseller, and user plugin contexts.
- Use DirectAdmin RAW endpoints for AJAX actions to avoid Evolution skin render issues.
- Package everything into a single installer shell script for easy deployment.

## Requirements

- A VPS or dedicated server with DirectAdmin installed.
- Root SSH access.
- DirectAdmin admin credentials.
- `bash` or POSIX `sh`, `tar`, `base64`, `awk`, `curl`, `sudo`.
- PHP available at `/usr/local/bin/php`.

The installer writes the plugin to:

```text
/usr/local/directadmin/plugins/zinstaller
```

## Quick Install

Download and run the installer as `root`:

```bash
wget -O zinstaller-installer.sh https://raw.githubusercontent.com/OWNER/REPO/main/zinstaller-installer.sh
chmod +x zinstaller-installer.sh
./zinstaller-installer.sh
```

During installation, the script asks for the DirectAdmin admin password and creates:

```text
/usr/local/directadmin/plugins/zinstaller/config/zinstaller.conf
```

You can also run it non-interactively:

```bash
DA_USER=admin DA_PASS='DIRECTADMIN_ADMIN_PASSWORD' ./zinstaller-installer.sh
```

Optional variables:

```bash
DA_URL='http://127.0.0.1:2222'
DA_USER='admin'
DA_PASS='your-directadmin-admin-password'
DEFAULT_OWNER='admin'
```

## DirectAdmin URLs

After installation, open DirectAdmin and use one of these URLs:

```text
/admin/plugins/zinstaller
/user/plugins/zinstaller
```

Legacy plugin URLs are also supported:

```text
/CMD_PLUGINS_ADMIN/zinstaller
/CMD_PLUGINS/zinstaller
```

## Updating

Run the same installer again:

```bash
./zinstaller-installer.sh
```

The installer backs up the existing plugin directory before replacing it:

```text
/usr/local/directadmin/plugins/zinstaller.bak-YYYYMMDDHHMMSS
```

By default, it preserves the existing config and runtime status files.

To force replacing the config:

```bash
FORCE_CONFIG=1 DA_USER=admin DA_PASS='DIRECTADMIN_ADMIN_PASSWORD' ./zinstaller-installer.sh
```

To force replacing runtime status files:

```bash
FORCE_RUNTIME=1 ./zinstaller-installer.sh
```

## What Is Included

The packaged installer includes only the ZInstaller plugin source:

- `admin/`
- `user/`
- `reseller/`
- `bin/`
- `config/zinstaller.conf.example`
- `hooks/`
- `images/`
- `scripts/install.sh`
- `plugin.conf`

The package does not include:

- Website files.
- Customer data.
- Databases.
- DirectAdmin users.
- Runtime logs.
- Existing plugin config containing passwords.

## Security Notes

- Do not commit `config/zinstaller.conf` to a public repository.
- Do not hard-code real DirectAdmin passwords into the installer.
- Prefer a private GitHub repository if this plugin is intended for paying customers only.
- If using a public repository, anyone with the raw URL can download the installer.

## Build Installer

If you modify the source and want to rebuild the single-file installer, run:

```bash
python tools/build-zinstaller-installer.py
```

The generated installer will be written to:

```text
dist/zinstaller-installer.sh
```

Upload that file to GitHub and use the raw GitHub URL for `wget`.

## Uninstall

To remove the plugin from a server:

```bash
rm -rf /usr/local/directadmin/plugins/zinstaller
rm -f /etc/sudoers.d/zinstaller-directadmin
```

This removes only the plugin. It does not remove WordPress sites created by the plugin.

