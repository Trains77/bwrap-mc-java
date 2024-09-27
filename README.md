# bwrap-mc-java

Tries to sandbox Minecraft in a per instance way in PrismLauncher

- Does not work with Flatpak version

The idea of the script is to provide stronger sandboxing in Minecraft instances by sandboxing only Minecraft itself, and not the launcher, and takes advantage of PrismLauncher's Wrapper command option to do so

The script relies on bwrap for sandboxing, slirp4netns for networking, and sommelier for *sandboxed* X11 sharing

Put this script as the Wrapper command in PrismLauncher 

Do note this script is made primarily made for my personal use, so there are some things in it that I use that you may want to change/remove

- Sandbox assumes by default that the vpn interface is wg0-mullvad, and non-vpn interface is wlan0, you can set these 2 variables near the top of the script
- The current users uid is by default mapped to 3581 in the sandbox, which shouldn't break anyhting but you can change it to be the user's normal uid near the top of the script
- Discord's socket is not bound in the sandbox, you would need to add that argument yourself
- Some versions or mods may not work with the sandbox, such as the Essentials mod
- Tries to load a seccomp filter from ~/.sandboxing/seccomp_default_filter.bpf

## Variables

Additionally, the scripts accepts settings in the form of environment variables set within the launcher, the following are:

AUDIO_BACKEND - Can be "pipewire", "pulseaudio", "none", or "both" defines which audio system to share with the sandbox, defaults to "none"

DISABLE_LOOPBACK - Can be either "true" or "false", defines Whether or not the sandbox should be able to reach the host's loopback address, defaults to "true" 

DISABLE_NETWORKING - Can be either either "true" or "false", defines whether or not the sandbox should get any networking, defaults to false

SHARE_X11 - Can be "true", "sommelier", or "false". Defines whether or not the sandbox should get direct access to the X11 server, a sandboxed X11 server through Sommelier, or no X11 server at all, defaults to false

MORE_VERBOSE - Does nothing, defaults to false

VPN_CONNECT_ONLY - Can be either "true", "force-disabled", or "false", Defines if Minecraft should be forced to use a vpn connection, force-disabled tries to force the sandbox to *not* use the vpn, defaults to false

BIND_HOME_MINECRAFT_FOLDER_ITEMS - If the following folders in ~/.minecraft should be bound into the sandbox: screenshots, saves, shaderpacks, optionsof.txt, and read-only resourcepacks, useful if you want to share these folders between instances, defaults to false

BIND_ADDITIONAL_FOLDERS - Mount additional space-seperated files and folders into the sandbox, does not support paths with a space in them

BIND_ADDITIONAL_FOLDERS_RO - Same as BIND_ADDITIONAL_FOLDERS but bind as read only
