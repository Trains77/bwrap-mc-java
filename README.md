# bwrap-mc-java

Tries to sandbox Minecraft in a per instance way in PrismLauncher

- Does not work with Flatpak version of the launcher

The goal of the script is to provide stronger sandboxing by isolating just the Minecraft game itself, and not the launcher

The script requires bwrap, optionally slirp4netns for Network isolation, can also optionally use Sommelier for X11 sandboxing,

To use the script, set it to be the wrapper command in your launcher's settings

![image](Screenshots/screenshot1.png)

Do note this script is made primarily made for my own personal use, so there are some things in it that I use/want that you may want to change or remove

- Sandbox assumes by default that the vpn interface is wg0-mullvad, and non-vpn interface is wlan0, you can set these 2 variables near the top of the script
- Instances whose paths contains a space will not work
- Some versions or mods may not work with the sandbox
	+ Mods that self update might not work 
- The script will try to load a seccomp filter from ~/.sandboxing/seccomp_default_filter.bpf if it exists
- If slirp4netns is not installed, you need to set NETWORK_DIRECT_CONNECT to true in order for Minecraft to access the internet

## Variables

Additionally, the scripts accepts settings in the form of environment variables set within the launcher, the following are:

AUDIO_BACKEND - Can be "pipewire", "pulseaudio", "none", or "both" defines which audio system to share with the sandbox, defaults to "none"

ENABLE_DISCORD - Can be either "true" or "false", when true, tries to bind $XDG_RUNTIME_DIR/discord-ipc-0 to the sandbox, useful if you want Discord Rich Presence, if $XDG_RUNTIME_DIR/discord-ipc-0 is a symlink, the script will attempt derefernce links and bind the actual socket file, defaults to "false"

DISABLE_LOOPBACK - Can be either "true" or "false", defines Whether or not the sandbox should be able to reach the host's loopback address, does not work if NETWORK_DIRECT_CONNECT is true, defaults to "true" 

DISABLE_NETWORKING - Can be either either "true" or "false", defines whether or not the sandbox should get any networking, defaults to false

NETWORK_DIRECT_CONNECT - If Minecraft should have access to all network interfaces like an unsandboxed program would, requires DISABLE_NETWORKING to be false.

PERSISTANT_CACHE - If the cache folder in the Minecraft directory should be persistant, defaults to false

SHARE_X11 - Can be "true", "sommelier", or "false". Defines whether or not the sandbox should get direct access to the X11 server, a sandboxed X11 server through Sommelier, or no X11 server at all, defaults to false

MORE_VERBOSE - Does nothing, defaults to false


VPN_CONNECT_ONLY - Can be either "true", "force-disabled", or "false", Defines if Minecraft should be forced to use a vpn connection, force-disabled tries to force the sandbox to *not* use the vpn, defaults to false

BIND_HOME_MINECRAFT_FOLDER_ITEMS - If the following folders in ~/.minecraft should be bound into the sandbox: screenshots, saves, shaderpacks, optionsof.txt, and read-only resourcepacks, useful if you want to share these folders between instances, defaults to false

BIND_ADDITIONAL_FOLDERS - Mount additional space-seperated files and folders into the sandbox, does not support paths with a space in them

BIND_ADDITIONAL_FOLDERS_RO - Same as BIND_ADDITIONAL_FOLDERS but bind as read only
