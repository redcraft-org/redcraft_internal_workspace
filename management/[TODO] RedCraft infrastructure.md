# [TODO] RedCraft infrastructure

[![hackmd-github-sync-badge](https://hackmd.io/l0_aF4dGTy2VXcL0P9RBag/badge)](https://hackmd.io/l0_aF4dGTy2VXcL0P9RBag)


[TOC]

## Instances

### How to get consistent infrastructure
- Using images generated by packer
- Using terraform files to describe servers

### How to monitor servers
- Using prometheus
- https://www.scaleway.com/en/docs/configure-prometheus-monitoring-with-grafana/
- This could be useful? https://github.com/scaleway/prometheus-scw-sd

## Minecraft server

### How to deploy servers
- Python script that deploys a Scaleway instance with a preconfigured image and restores the last snapshot of the disk, ssh to the server and run the update script
- Golang runner to handle the console (rcsm)

### How to configure servers and plugins
- GitHub repo with a `common` folder containing all config and a `plugins.json` file containing the list of plugins on S3, and the same for each server if necessary
- GitHub Actions to create `.tar` files for each server containing a `plugins` directory with config and `plugins.json` containing a list of plugins to download from an endpoint
- Update script that downloads the `.tar` archive for the current server and parses `plugins.json` and download the latest compatible versions

### How to update server engine jar and plugins
- Create an update center that will automatically download updates
- Needs to be able to download from SpigotMC.org
- Needs to be able to download from Jenkins
- Needs to be able to download the last release from a GitHub project
- Needs to be able to compile any .pom from a GitHub project
- Needs to be able to download any file from any URL

#### Handle breaking updates
- Flag in a metadata file with all the plugin versions such as 1.2.x
- To flag a breaking update, it's when a plugin goes from 1.2.4 to 1.3 for example
- Automatically create a new branch with a pull request to allow the breaking update to be installed, and people can edit the branch if there's any configuration necessary

### How to handle planned restarts
- Custom plugin (RedCraftReboot)
    - `/reboot <minutes> <reason>`
    - If minutes set to 0, start the countdown from 10 sec
    - Max minutes should be 15
    - Broadcast who asked for a reboot and why when ran except if console without args
    - Args not needed for console, default to 15 min and "Scheduled daily reboot"
    - Warn in the tchat 15 min, 10 min, 5 min, 4 min, 3 min, 2 min before reboot
    - Start a countdown in the bossbar until reboot
    - Do a countdown from 10 to 0 as title and end with "Reboot!"

### How to handle roles and donation list
- MySQL DB
- Dispatch command to reload Lucky Perms on all servers

### How to handle cross server tchat
- Custom plugin (RedCraftChat)
    - Discord bridge using webhooks - show player head and name in the webhook
    - At first join, ask the user in the tchat which langages they speak, they will not be able to post in the tchat before they click which langages they use, can be done in a fake chest using banners for flags
    - Assume the user speaks their country IP language (main one) + their MC language version if we can get it
    - Translate messages comming back from the server directly on Bungee (do prefix detection for tchat)
    - Spell check for messages (re use parts of KingChat)

### How to handle multilang translations for plugins
- Put it in Deepl and pray? :warning: Might not be the best idea

### How to handle plugin saved data
- Avoid plugins with saved data in the config dir

### How to handle backups

- Using [scheduled serverless](https://developers.scaleway.com/en/products/serverless/api/#cron) on Scaleway or [rundeck](https://www.rundeck.com/open-source)
- Stored on S3
- Add S3 rules to delete old backups

#### Worlds
- Snapshot disk, restore it on a temporary instance with some CPU horsepower, use `lbzip2` and upload to S3

#### Inventories
- Do a simple .tar.gz directly on the server every 10 minutes

#### Plugin data
- Avoid plugins with saved data in the config dir
- If we do have plugin data, save them every hour with a simple `.tar.gz`


### How to handle world renders
- Using Chunky for an overview of the map
- Using Overviewer for detailed version of the map
- Chunky should be rendered once a week because it's load intensive
- Overviewer should run every hour or so
- Stored on attached block storage
- Served by apache or nginx


### How to give perks to good players

Use medals!

Medals can be either given by mods for skills (redstoner, engineer, builder, architect) or for playtime (suviver, veteran) and can give permissions.

Any new player would be a newbie, and having at least one medal would make you a member

- Newbie
    - Member
        - Redstoner
            - Engineer
        - Builder
            - Architect
        - Surviver
            - Veteran
    - VIP
        - KingVIP/RedVIP

    - Staff
        - Helper
            - Modo
                - Admin
                    - Founder

## Discord server

### What to sync
- Roles
- Username
- UUID on note for some sort of quick match

### How to set roles
- Type a command in game to get a link to connect to Discord
- Generate a channel on a different Discord server
- Give an invite to the player in the tchat
- Wait for the user to log in
- When the user joins the channel, get info and kick the user from the temp server
- If the Discord account ID can't be found on the main server, send an invite to the user
- Otherwise, trigger sync

### Sync triggers
- New language selected
- Primary language changed
- Account joined Discord
- Account joined Minecraft
- Command in game
- Permission event in game
- Cron task

### How to handle multilang
- Bot with reactions with flags to give you a role with your lang, giving you access to some channels in your language
- NB: reactions can also be used for each server (creative, redstone, survival, factions, etc...)
- For the Minecraft bridge (RedCraftChat), use Deepl to do realtime translation to a #minecraft-fr channel and #minecraft-en channel for example

## Language support

### When to add support for a language?
- The language selector bot will have reactions for most common languages
- We can map users that want a language with the Minecraft server and check how many active players speak that language
- If there's at least one player speaking a language, we'll translate to that player ingame, but not on Discord
- If there are at least 10 active players speaking a language, a new channel for live tchat will be created automatically on Discord and users will be notified of the creation of the channel from the bot via PM
- If the user adds a reaction when the channel doesn't exist, they will get a PM to explain them that there's not enough users to justify the cost of translation
- VIP counts as two active players
- KingVIP counts as three active players
- If the count goes bellow 5 active players speaking a language, the channel will be deleted and the active users will be notified via PM