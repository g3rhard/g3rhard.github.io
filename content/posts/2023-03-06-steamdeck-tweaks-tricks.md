---
layout: post
title:  "Steamdeck: Tweaks & Tricks"
date:   2023-03-06 09:00:00 +0800
categories: nix cli
---

## Setting up password for sudo

- Just run "passwd" command to setup the password, it will be needed for future installations

  ```sh
  passwd
  ```

## Install Heroic Games Launcher as alternative for Epic Games Store

- Switch to "Desktop Mode"
- Open "Discover" app to find application
- Install "Heroic Games Launcher"

## Install ProtonUp-Qt utility to control Proton versions

- Switch to "Desktop Mode"
- Open "Discover" app to find application
- Install "ProtonUp-Qt"
- After installation you will be able to setup Proton version for different games launchers (Steam/Heroic)
- TODO: Transfer save data for Heroic games

## Install CryoUtilities tweaks

And last, but not least - CryoUtilities

> Scripts and utilities to improve performance and manage storage on the Steam Deck.

- Option 1: Download [shortcut](https://raw.githubusercontent.com/CryoByte33/steam-deck-utilities/main/InstallCryoUtilities.desktop) from GitHub and run it from download location
- Option 2: Use installer via terminal

  ```sh
  curl https://raw.githubusercontent.com/CryoByte33/steam-deck-utilities/main/install.sh | bash -s --
  ```

That's all.

## Additional links

1. [CryoByte33/steam-deck-utilities](https://github.com/CryoByte33/steam-deck-utilities)
2. [Heroic Games Launcher](https://heroicgameslauncher.com)
    - [Heroic-Games-Launcher/HeroicGamesLauncher](https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher)
3. [Wagner's TechTalk - Steam Deck Guide](https://wagnerstechtalk.com/steamdeck)
