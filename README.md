# bwrap-mc-java

Tries to sandbox Minecraft in a per instance way in PrismLauncher

The idea of the script is to provide stronger sandboxing in Minecraft instances by sandboxing only Minecraft itself, and not the launcher, and takes advantage of PrismLauncher's Wrapper command option to do so

The script relies on bwrap for sandboxing, slirp4netns for networking, and sommelier for X11 sharing

Most likely does not work with Flatpak versions of the launcher

Put this script as the Wrapper command in PrismLauncher 

Do note this script is made primarily made for my personal use, so there are some things in it that I use that you may want to change/remove

- Sandbox assumes by default that the vpn interface is wg0-mullvad, and non-vpn interface is wlan0, you can set these 2 variables near the top of the script
- Discord's socket is not bound in the sandbox, you would need to add that argument yourself
- Some versions or mods may not work with the sandbox
- Tries to load a seccomp filter from ~/.sandboxing/seccomp_default_filter.bpf

Additionally, the scripts accepts settings in the form of environment variables set within the launcher, the following are:

AUDIO_BACKEND - Can be either "pipewire", "pulseaudio", "none", or "both" defines which audio system to share with the sandbox, defaults to "none"

DISABLE_LOOPBACK - Can be either "true" or "false", defines Whether or not the sandbox should be able to reach the host's loopback address, defaults to "true" 

DISABLE_NETWORKING - Can be either either "true" or "false", defines whether or not the sandbox should get any networking, defaults to false

ENABLE_X11 - Can be either "true" or "false", Defines, whether or not the sandbox should get access to a X11 server via Sommelier, defaults to false

MORE_VERBOSE - Does nothing, defaults to false

VPN_CONNECT_ONLY - Can be either "true", "force-disabled", or "false", Defines if Minecraft should be forced to use a vpn connection, force-disabled tries to force the sandbox to *not* use the vpn, defaults to false

BIND_HOME_MINECRAFT_FOLDER_ITEMS - If the following folders in ~/.minecraft should be bound into the sandbox: screenshots, saves, shaderpacks, optionsof.txt, and read-only resourcepacks, useful if you want to share these folders between instances, defaults to false

BIND_ADDITIONAL_FOLDERS - Mount additional space-seperated files and folders into the sandbox, does not support paths with a space in them

BIND_ADDITIONAL_FOLDERS_RO - Same as BIND_ADDITIONAL_FOLDERS but bind as read only
