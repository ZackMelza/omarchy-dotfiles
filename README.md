# Omarchy dotfiles

This repository stores Zack's personal Omarchy/Hyprland overrides. Omarchy's
packaged defaults remain managed by Omarchy; this repository contains only the
user files that should be restored after a fresh installation.

## Files to keep backed up

The reusable configuration is under `omarchy/hypr/`:

- `hyprland.lua` — loads the personal modules, including workspace rules.
- `input.lua` — US/Greek keyboard layouts, Alt+Shift switching, and normal Caps Lock behavior.
- `bindings.lua` — personal keybindings and keybindings-help descriptions.
- `workspace_rules.lua` — application-to-workspace placement rules.

The reusable Zsh configuration is under `zsh/`:

- `.zshrc` — loads Omarchy's user environment setup.
- `starship.toml` — full-path prompt and Git status/counts.

The custom Omarchy theme is under `omarchy/themes/symbiote/`:

- `colors.toml` — black-suit palette with cool web highlights and crimson accents.
- `backgrounds/` — matching wallpapers, including the original
  `marvel-black-suit.webp` reference image plus `symbiote.png`, `city-web.png`,
  `black-weave.png`, and `moonlit-rooftop.png`.

Current application placement includes browsers on workspace 2, VS Code and
other IDEs on 3, Steam/Lutris/Heroic on 5, Discord and messaging apps on 7,
and games on 8. Email, screen sharing, virtual machines, and multimedia are
also assigned to workspaces 1, 4, 6, and 9 respectively.

## Restore on a new Omarchy installation

Install and launch Omarchy once first so its normal `~/.config/hypr/` files
exist. Then run:

```bash
sudo pacman -S zsh
chsh -s /usr/bin/zsh

git clone https://github.com/ZackMelza/omarchy-dotfiles.git ~/dotfiles
mkdir -p ~/.config/hypr

# Optional safety backup of the fresh/current user config.
cp -a ~/.config/hypr ~/.config/hypr.backup

cp ~/dotfiles/omarchy/hypr/hyprland.lua ~/.config/hypr/
cp ~/dotfiles/omarchy/hypr/input.lua ~/.config/hypr/
cp ~/dotfiles/omarchy/hypr/bindings.lua ~/.config/hypr/
cp ~/dotfiles/omarchy/hypr/workspace_rules.lua ~/.config/hypr/
cp ~/dotfiles/zsh/.zshrc ~/.zshrc
cp ~/dotfiles/zsh/starship.toml ~/.config/starship.toml

luac -p ~/.config/hypr/hyprland.lua
luac -p ~/.config/hypr/input.lua
luac -p ~/.config/hypr/bindings.lua
luac -p ~/.config/hypr/workspace_rules.lua

hyprctl reload
hyprctl configerrors
```

After restoring, the personal shortcuts are:

| Shortcut | Action |
| --- | --- |
| `Super+H` | Keybindings help |
| `Super+E` | File manager |
| `Super+L` | Lock system |
| `Super+D` | Discord |
| `Super+Shift+C` | Codex |

The ThinkPad F1 mute key remains available while locked and uses the custom
audio-sink mute handler.

`Super+C` remains Omarchy's Universal Copy binding.

## Updating the backup

After changing a user config file, copy it back into this repository and
commit it:

```bash
cp ~/.config/hypr/<changed-file>.lua ~/dotfiles/omarchy/hypr/
git -C ~/dotfiles add omarchy/hypr/<changed-file>.lua
git -C ~/dotfiles commit -m "Describe the customization"
git -C ~/dotfiles push
```

## Important boundaries

- Do not edit or copy `/usr/share/omarchy/`; Omarchy owns and updates it.
- Do not copy the entire `~/.config/hypr/` directory over a new install.
- Do not run the JaKooLit/Hyprland repository setup script on Omarchy. That
  repository is reference material for selected workspace behavior only.
- Keep machine-specific monitor, display, and generated state out of this
  backup unless they are deliberately made portable.
