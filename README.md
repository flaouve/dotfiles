# dotfiles ~~(o)~~

My personal Ansible-based workstation setup for Fedora. Fresh install to fully configured in one command.

```
ansible-playbook --ask-vault-pass -K playbook.yml -i hosts.ini
```

---

## what it does

```
[1] debloat      -- removes gnome bloat, firefox, libreoffice and other garbage
[2] repos        -- RPM Fusion, Terra, NVIDIA, Chrome, VS Code, Antigravity
[3] install      -- git, podman, helix, rust, go, btop, steam, zed editor and more
[4] flatpak      -- sets up flathub and installs apps (vesktop, librewolf, bottles...)
[5] nvidia       -- auto-detects GPU and installs drivers + container toolkit
[6] dotfiles     -- copies bashrc, vimrc, gitconfig, helix config, dnf config
[7] wallpaper    -- sets wallpaper via gsettings (GNOME only)
[8] systemd      -- enables tailscaled
[9] ssh + gpg    -- deploys keys from vault (hostname: cutie only)
```

---

## structure

```
dotfiles/
|- assets/
|  |- wallpaper/       <- wallpaper files
|  |- avatar/
|- home/               <- user dotfiles (bashrc, vimrc, gitconfig, helix)
|- system/             <- system configs (dnf.conf, resolved.conf)
|- roles/              <- ansible task files
|- vars/
|  |- secrets.yml      <- ansible-vault encrypted (ssh + gpg keys)
|- hosts.ini
|- playbook.yml
```

---

## secrets setup

keys are stored encrypted in `vars/secrets.yml` via ansible-vault. never touch the disk unencrypted (except briefly in `/tmp` during GPG import).

create the vault file:

```bash
ansible-vault create vars/secrets.yml
```

format inside:

```yaml
ssh_private_key: |
  -----BEGIN OPENSSH PRIVATE KEY-----
  ...
  -----END OPENSSH PRIVATE KEY-----

ssh_public_key: "ssh-rsa AAAA... user@host"

gpg_private_key: |
  -----BEGIN PGP PRIVATE KEY BLOCK-----
  ...
  -----END PGP PRIVATE KEY BLOCK-----

violet_one_tailscale_key: "tskey-auth-bla bla bla"

cutie_tailscale_key: "tskey-auth-bla bla bla"
```

---

## requirements

- Fedora (tested on Fedora 44+)
- Ansible 2.20.6+
- hostname must be `cutie` for SSH/GPG/dotfiles tasks

```bash
sudo dnf install -y ansible
```

---

## running

full setup:

```bash
ansible-playbook --ask-vault-pass -K playbook.yml -i hosts.ini
```

single task:

```bash
ansible-playbook --ask-vault-pass -K playbook.yml -i hosts.ini --start-at-task "enable ssh key"
```

---

## 🔌 GNOME Extensions

The following extensions must be installed **manually** from the GNOME extensions store or their respective sources. Due to GNOME's restrictions, these extensions cannot be included in the automated installation script.

> 💡 You can install extensions via [extensions.gnome.org](https://extensions.gnome.org) or the **Extension Manager** app.

| # | Extension Name | Description |
|---|---------------|-------------|
| 1 | **AppIndicator and KStatusNotifierItem Support** | Displays application icons (AppIndicator) in the system tray |
| 2 | **Battery Health Charging** | Protects battery life by preventing charging above a set percentage |
| 3 | **Bluetooth Battery Meter** | Shows Bluetooth device battery levels in the system tray |
| 4 | **Blur my Shell** | Adds blur effects to the app launcher, panel, and lock screen |
| 5 | **Caffeine** | Prevents the screen from auto-locking and the system from sleeping |
| 6 | **Clipboard Indicator** | An advanced clipboard manager for managing clipboard history |
| 7 | **Color Picker** | A color picker tool to grab the color of any pixel on the screen |
| 8 | **Compiz-alike Windows Effects** | Adds Compiz-like animations and effects to window movements |
| 9 | **Dash to Dock** | Transforms the app launcher (dash) into an always-visible dock |
| 11 | **GSConnect** | Integrates Android devices via the KDE Connect protocol |
| 12 | **Just Perfection** | Customizes GNOME Shell elements (panel, activities, location bar, etc.) |
| 14 | **Quick Settings Audio Panel** | Adds an advanced audio control panel to the quick settings menu |
| 16 | **Restart to...** | Adds boot options (UEFI, Windows, etc.) to the power menu |
| 17 | **Tiling Shell** | Smart window tiling support; snaps windows into zones |
| 18 | **User Avatar in Quick Settings** | Displays the user's avatar photo in the quick settings menu |
| 20 | **Weather O'Clock** | Shows weather information alongside the clock in the top panel |
| 21 | **Wiggly** | Adds a fun slow wobble animation when dragging windows |


---

## notes

- NVIDIA driver install is automatic -- skipped if no GPU detected
- Flatpak apps install as user (no root)
- GPG import shows `rc=2` warning from gpg itself -- this is normal, key imports fine
- SSH keypair is deployed to `~/.ssh/id_rsa` + `id_rsa.pub` for GNOME Keyring compatibility

---

```
(^_^) works on my machine
```
