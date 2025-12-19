# The macOS Game Workaround Repo
A centralized list of every known way to make games run on macOS

## Table of Contents

1. [Native Mac Games](#native-mac-games)
   - [Steam's Handling of 32-bit Mac Games](#steams-handling-of-32-bit-mac-games)
2. [iOS Versions](#ios-versions)
3. [Modern Windows Games](#modern-windows-games)
   - [Dual Booting](#dual-booting)
   - [Translation Layers](#translation-layers)
      - [Handling AVX and F16C](#avx-f16c)
   - [Virtualization](#virtualization)
4. [Games Available on Other Platforms](#games-available-on-other-platforms)
   - [Multiple Platform Emulators](#multiple-platform-emulators)
   - [Sony PlayStation Series](#sony-playstation-series)
   - [Xbox Series](#xbox-series)
   - [Nintendo Consoles](#nintendo-consoles)
   - [Android](#android)
   - [MS-DOS Games](#ms-dos-games)
   - [Windows 95-98](#windows-9598)
   - [MacOS 9](#MacOS-9)
5. [Cloud Gaming](#cloud-gaming)
6. [Local Area Streaming](#local-area-streaming)
7. [Game Engine Ports, Hacks, and Patches](#game-engine-ports-hacks-and-patches)
    - [Unity and Adobe Air Games](#unity-and-adobe-air-games)
    - [RPG Maker games](#RPG-maker-games)
    - [Mac Source Ports](#mac-source-ports)
    - [ScummVM](#scummvm)
    - [Nintendo 64 Recompilations](#nintendo-64-recompilations)
    - [Sonic ports](#sonic-ports)
    - [Individual Mac Ports](#individual-mac-ports)
8. [Making VR work](#making-vr-work)

## <a id="native-mac-games"></a>Native Mac Games

Native Mac games and ports are, most of the time, the best way to enjoy a game on your Mac. They are compiled with  in-mind and the developer presumably offers support for running them on macOS. There are a few different places to find these games:
- Mac App Store: Apple's main app store which has some exclusive Mac games. Some are only available with an Apple Arcade subscription. The Mac App Store sometimes offers Universal Purchase, which means you can play the game on every supported Apple platform for a single fee
- [GOG](https://www.gog.com/): Unlike the other stores, GOG provides games without DRM
- [Epic Games Store](https://store.epicgames.com/): Notable for offering free games each Thursday, although Mac games are quite rare (many games that offer a Mac version are only available for Windows on the Epic Games Store)
- [Steam](https://store.steampowered.com/): The store with the largest game selection by far
- [MacGameStore](https://www.macgamestore.com/): More centered around casual games; it has been a staple of Mac gaming for decades

> [!TIP]
> When you buy a game on GOG, the Epic Games Store or Steam, you also get a license for the Windows version, and Linux version when available.

### <a id="steams-handling-of-32-bit-mac-games"></a>Steam's Handling of 32-bit Mac Games

When Apple decided to drop 32-bit support with macOS Catalina, many Mac games became non-functional overnight. The games could still be played provided you didn't update macOS but since Steam eventually also dropped support for 32-bit on macOS, there is no way to run these games natively on macOS anymore.

Valve decided to make developers manually specify if their game was 64-bit in the SteamWorks database, making games 32-bit by default. Games assumed to be 32-bit are signaled as incompatible with macOS Catalina and newer on the game's page. They are also signaled in the game library by an icon (🚫) next to their name and a message on their description page. These games are filtered out when clicking on the Apple logo to filter the Mac-only games. Unfortunately, many 64-bit games are incorrectly assumed to be 32-bit. Some of them are listed [here](https://docs.google.com/spreadsheets/d/17DkOsI9AwAT4dzPkLmunYJJmUpf1FuWR62Q1vAEfJzM/edit?usp=sharing).

Additionally, games that were delisted from the store are all reported as 64-bit in the library, whether the database says so or not.

## <a id="ios-versions"></a>iOS Versions

Some Windows games have also been released for iPhone and iPad, and those apps can technically run on Apple Silicon-based Macs when allowed by their publishers on the App Store, which is arguably the closest thing to a native macOS version. However, many games are simply not allowed to run. There is a workaround for this with [PlayCover](https://playcover.io/) and [Sideloadly](https://sideloadly.io/), which are free apps that allow sideloading and running decrypted iOS/iPadOS apps on Apple Silicon-based Macs.
For 32 bits games that won't work with PlayCover, there is an emulator for the early iPhones called [touchHLE](https://github.com/touchHLE/touchHLE/releases).

## <a id="modern-windows-games"></a>Modern Windows Games

### <a id="dual-booting"></a>Dual Booting

#### Windows (Intel-Only)
Intel-based Macs can boot Windows like any other PC thanks to [Boot Camp](https://support.apple.com/us-en/guide/bootcamp-control-panel/bcmp29b8ac66/mac). This will run any Windows game.

> [!NOTE]
> Apple Silicon-based Macs cannot boot Windows, though Apple has said it is open to making Boot Camp available to run Windows for ARM, pending Microsoft's approval.

#### Linux
Additionally, Intel-based Macs can dual-boot on [Batocera](https://batocera.org/), a Linux distribution centered around launching emulators and ROMs with a dedicated interface, and Apple Silicon based Macs can dual-boot on [Asahi Linux](https://asahilinux.org/), which allows to run Windows games with Proton right from within Steam.

### <a id="translation-layers"></a>Translation Layers

For those who want to run Windows games (with no native port/iOS version) on their Apple-silicon hardware (and not through a cloud provider), this is likely the best option. Modern windows games (from this century) are going to be x86, Windows, likely [DirectX](https://en.wikipedia.org/wiki/DirectX)-based titles. This means that there are multiple translation layers necessary to play on an ARM (Apple Silicon), macOS, [Metal](https://en.wikipedia.org/wiki/Metal_(API)) machine:
- [Apple silicon only] Rosetta 2: This translates x86 into ARM instructions
- Operating system layer: [Wine](https://www.winehq.org/) translates Windows API instructions into macOS API instructions on the fly
- Graphics layers: : These translate the GPU instructions from DirectX API to Apple's Metal graphics API
   - [D3DMetal](https://developer.apple.com/games/game-porting-toolkit/): DirectX 11/12 to Metal
   - [DXMT](https://github.com/3Shain/dxmt): DirectX 10/11 to Metal
   - [VKD3D](https://gitlab.winehq.org/wine/vkd3d/-/wikis/home): DirectX 12 to Vulkan and then through [MoltenVK](https://github.com/KhronosGroup/MoltenVK) to Metal
   - [DXVK](https://github.com/Gcenx/DXVK-macOS): DirectX 10/11 to Vulkan and then through [MoltenVK](https://github.com/KhronosGroup/MoltenVK) to Metal
   - [WineD3D](https://fdossena.com/?p=wined3d/index.frag): DirectX 1-11 to OpenGL

> [!NOTE]
> DXVK used on Mac is a special macOS version based off an older version of the DXVK used in Linux. Similarly, VKD3D is only available through CrossOver (see below) at the moment because they have ported a version to use with macOS.

Unless you are keen on building everything from source and integrating the layers yourself, you are best served using one of the following programs that combine them:

- [CodeWeavers CrossOver](https://www.codeweavers.com/crossover/) (paid, see footnote)[^1]
- [Sikarugir](https://github.com/Sikarugir-App/Sikarugir) (successor of Wineskin and Kegworks, free and open source)
- [Heroic Games Launcher](https://heroicgameslauncher.com/) (free and open source)
- [Porting Kit](https://www.portingkit.com/) (free)
- [Game Porting Toolkit](https://developer.apple.com/games/game-porting-toolkit/) (command-line interface only, free)
- [Wine](https://www.winehq.org/) (command-line interface only, free and open source)
- [ChooChoo Loader](https://github.com/wowitsjack/choochoo-loader/)[^2]

> [!NOTE]
> While this solution doesn't make every Windows game work flawlessly (most notably, games with anti-cheat protection won't run), it's usually the best option to try first.

[^1]: CrossOver usually offers the best compatibility and most up-to-date versions of Wine and the various DirectX translation interfaces. **Codeweavers is also the biggest contributor to the Wine project, so supporting them helps everyone, especially because almost all the other projects use CrossOver's Wine sources.** CrossOver also comes with CrossTies, a list of game-specific fixes and workarounds for games that don't run out of the box through Wine, but starting with CrossOver 25 this will be phased out due to a better general compatibility. A common misconception about CrossOver is that the fee only allows you to use it for a year, whereas in reality you can use it forever, but you only get a year's worth of free updates. Historically, they have had a good sale around Cyber Monday. [CXPatcher](https://github.com/italomandara/CXPatcher) is a non-official tool that allows updating CrossOver's dependencies, such as updated graphics layers.
[^2]: For injecting trainers, cheats, and debuggers, one can use the ChooChoo Loader Engine to act as a pre-loader.
This ensure all processes are able to see and interact with each other in a Wine gaming enviroment.

If you want to know if a specific game runs, there are compatibility lists:
- [for CrossOver](https://www.codeweavers.com/crossover/#compatibility)
- [for Porting Kit](https://www.portingkit.com/games)
- https://www.applegamingwiki.com/wiki/Home
- https://macgamingdb.app/

Sometimes, the Windows version of specific store launchers (GOG Galaxy, Epic Games Launcher, Amazon Games) can get updates which break support with the above software translation layers. A workaround is to use [Heroic Games Launcher](https://heroicgameslauncher.com/), a native launcher for Windows and Mac games for these stores. There is also a Mac-exclusive app called [Mythic](https://getmythic.app/), which so far only supports the Epic Games Store and only runs Windows games with its own embedded version of Wine, while Heroic also allows to run games with external versions of Wine such as CrossOver.

Finally, [Winetricks](https://github.com/Winetricks/winetricks) is a script (embedded in CrossOver, Sikarugir, and Heroic) which allows to install necessary dependencies (such as Microsoft DLLs and fonts), tweak settings and workarounds to make games work with Wine and its derivatives.

#### <a id="avx-f16c"></a>Handling AVX, AVX2, FMA and F16C
Some Windows games will sometimes require specific extentions of the Intel processors to run, such as AVX, AVX 2, FMA and F16C. The 3 first ones are handled by Rosetta 2 in macOS Sequoia, but you have to add ROSETTA_ADVERTISE_AVX=1 in the launch parameters of your game (in Steam, select your game properties, and paste it in the launch options field, at the bottom of the General tab). Note that as of CrossOver 25, this isn't necessary anymore as it handles it automatically. Apple added support for F16C instructions to Rosetta 2 as of macOS 15.4. Users of previous versions of macOS can apply individual patches on specific games to work around the issue:
- [patch for God of War: Ragnarok](https://www.youtube.com/watch?v=eNKtbUVEgSU)
- [patch for Ghost of Tsushima](https://www.youtube.com/watch?v=zwJwlRHW3k4)
- [patch for Horizon Forbidden West](https://community.pcgamingwiki.com/files/file/3114-horizon-forbidden-west-f16c-инструкции-исправить/)

### <a id="virtualization"></a>Virtualization

Virtualization is different from emulation, in that it runs the native code on the host processor, when they share the same instruction set. The processor is shared between the two environments at once. For instance, a Nintendo Switch runs on an ARM processor, and thus can be virtualized on an Apple Silicon-based Mac. While there is an overhead compared to running a game on its original platform since the hardware is shared with macOS, it's generally faster than emulation as it converts one instruction set to the other. Unlike translation layers, these will run and require installing complete operating systems (like Windows or Linux) and are usually more focused on general-purpose computing than gaming.

- [Parallels Desktop](https://www.parallels.com/) (the only Microsoft-sanctioned solution to virtualize Windows for ARM, paid)
- [VMWare Fusion](https://blogs.vmware.com/teamfusion/2024/05/fusion-pro-now-available-free-for-personal-use.html) (free for personal use)
- [Oracle VirtualBox](https://www.virtualbox.org/) (free)
- [UTM](https://apps.apple.com/us/app/utm-virtual-machines/id1538878817) (free)

## <a id="games-available-on-other-platforms"></a>Games Available on Other Platforms

These games can be run either through emulation or virtualization depending on the processor of the platform it runs on. There are countless emulators available for macOS, supporting the most famous to the most obscure platforms, so this list won't be exhaustive and will only feature the most prominent emulators. Some Windows games were also released on these platforms (or inversely, some games first released on older platforms were eventually released on Steam through emulation), which provides an alternative, and sometimes superior, way of playing them on macOS. Some emulators provide improvements to how the original games ran, for instance by increasing their resolution, or even supporting bespoke HD graphical packs, effectively offering a "remastered" experience.

[Detailed "how-to" video](https://www.youtube.com/watch?v=hoWn4rS7Vxs) from Retro Game Corps for setting up various emulators on Mac

### <a id="multiple-platform-emulators"></a>Multiple Platform Emulators

- [OpenEmu](https://openemu.org/), a scarcely updated, very polished, Mac-only app that emulates many platforms
- [RetroArch](https://www.retroarch.com/), also available on [Steam](https://store.steampowered.com/app/1118310/RetroArch/) and the [Mac App Store](https://apps.apple.com/us/app/retroarch/id6499539433)
- [Ares](https://ares-emu.net/)
- [Nostlan](https://qashto.itch.io/nostlan) (paid): a front-end for multiple emulators focused on ease of use and automatic setup

### <a id="sony-playstation-series"></a>Sony PlayStation Series

- [ShadPS4](https://shadps4.net/) : PlayStation 4
- [RPCS3](https://rpcs3.net/) : PlayStation 3 ([compatibility list](https://rpcs3.net/compatibility))
- [AetherSX2](https://aethersx2.net/download-aethersx2-for-macs/) (discontinued and Apple Silicon only) and [PCSX2](https://pcsx2.net/) ([compatibility list](https://pcsx2.net/compat)): PlayStation 2
- [DuckStation](https://www.duckstation.org/) : PS1
- [Vita3K](https://vita3k.org) : PS Vita
- [PPSSPP](https://www.ppsspp.org/) : PSP

### <a id="xbox-series"></a>Xbox Series

- [Xemu](https://xemu.app/): Xbox
- [Xenia](https://xenia.jp/): Xbox 360, the emulator has no Mac port but can be run through Software Translation Layer ([video guide](https://www.youtube.com/watch?v=ELug8rz1rBg))

### <a id="nintendo-consoles"></a>Nintendo Consoles

- [Ryujinx](https://git.ryujinx.app/ryubing/ryujinx/-/releases): Switch
- [Cemu](https://cemu.info/): Wii U
- [Dolphin](https://www.dolphin-emu.org/): GameCube & Wii
- [Citra](https://citra-emulator.com/download) Nintendo 3DS

### <a id="android"></a>Android

> [!NOTE]
> This is technically virtualization and not emulation since Android runs on ARM processors just like Apple Silicon-based Macs.

- [Bluestacks Air](https://www.bluestacks.com/mac) (free)
- [NoxPlayer](https://www.bignox.com/) (Doesn't work on Apple Silicon, free with ads/subscription)
- [MumuPlayer](https://www.mumuplayer.com/mac/) (paid)

### <a id="ms-dos-games"></a>MS-DOS Games

- [Boxer](https://boxer.thec0de.com/) (turns MS-DOS games into macOS apps with an intuitive interface)
- [DOSBox](https://www.dosbox.com/)
- [DOSBox-X](https://dosbox-x.com/) (a more flexible version of DOSBox, with more features)
- [DOSBox Staging](https://www.dosbox-staging.org/) (a more modern version of DOXBox, complete with CRT emulation)
- [Dreamm](https://aarongiles.com/dreamm/) (a bespoke emulator for LucasArts games and some other miscellaneous MS-DOS games)

### <a id="windows-9598"></a>Windows 95/98
- [86Box](https://github.com/86Box/86Box/releases) : Low level harware PC emulator supporting several OSes, including BeOS & NextStep ([video tutorial](https://www.youtube.com/watch?v=xghrSaKn7yM))
- [MacBox for 86Box](https://github.com/Moonif/MacBox) : optional manager app for 86Box ([video guide](https://www.youtube.com/watch?v=QLRab9n8tm8))

### <a id="MacOS-9"></a>MacOS 9
Older versions of macOS X once allowed to run apps made for MacOS 9 in a dedicated environment called Classic. You can relive those days with emulators such as [Basilisk II](https://basilisk.cebix.net/), [SheepShaver](http://sheepshaver.cebix.net/) and even the classic black and white Macintosh of yore with [MiniVMac](https://www.gryphel.com/c/minivmac/).
Edward Mendelson even made a custom distribution of SheepShaver with additional features such as a shared folders and printer support, ready to use, called [Mac OS 9](https://www.mendelson.org/macos9osx.html). (thanks [u/HomeStarRunnerTron](https://www.reddit.com/user/HomeStarRunnerTron/)!)

## <a id="cloud-gaming"></a>Cloud Gaming

Cloud gaming runs games on a remote machine and streams the video output to your Mac in real-time, while your Mac just displays the video stream and sends controller inputs to the server. It requires a good internet connection, but it can sometimes be the only solution for specific games. Note that most of these only support a limited selection of games and require monthly payments.

- [NVidia GeForce Now](https://www.nvidia.com/en-us/geforce-now/). If you're having stuttering issues over WiFi, [here's how to fix it](https://www.reddit.com/r/GeForceNOW/comments/195l8ff/stuttering_issues_with_geforce_now_on_macos_over/?share_id=17Q90d4Szq3qDZnRlkYGN&utm_content=2&utm_medium=ios_app&utm_name=ioscss&utm_source=share&utm_term=1)
- [Xbox Cloud Gaming](https://www.xbox.com/en-US/cloud-gaming)
- [Amazon Luna](https://luna.amazon.com) (works in Google Chrome only, free weekly games with Amazon Prime)
- [Boosteroid](https://boosteroid.com/)
- [Shadow](https://shadow.tech/)
- [AntStream](https://www.antstream.com) (retro games)
- [Steam Link](https://apps.apple.com/us/app/steam-link/id1246969117) (Mac App Store link) : a selection of Steam games can be played online with friends with games they own and run on their PC.

## <a id="local-area-streaming"></a>Local Area Streaming

There are also local streaming solutions allowing you to play on your Mac a game running on another machine you own:

- [Steam Link](https://apps.apple.com/us/app/steam-link/id1246969117) (Mac App Store link) : a selection of Steam games can be run on your PC and played on your Mac. You can also play online with friends with games they own and run on their PC.
- [PS Remote](https://www.playstation.com/en-us/support/games/playstation-remote-play-on-pc-and-mac/): allows streaming games from your PS5 to your Mac
- [Moonlight](https://github.com/moonlight-stream/moonlight-qt): allows low-latency streaming from a NVIDIA GPU-powered PC to your Mac over your local network.
- [XBPlay](https://www.studio08.net/) is a third-party app allowing to remote play your Xbox One or Series X/S console as well as xCloud titles

## <a id="game-engine-ports-hacks-and-patches"></a>Game Engine Ports, Hacks, and Patches

### <a id="unity-and-adobe-air-games"></a>Unity and Adobe Air Games

It is possible to make a selection of Windows and Mac 32-bit games made with Unity or Adobe Air run on recent Macs, on occasion even as Universal Binaries.

- [Adobe Air games](https://m0rekz.github.io/blogspot/air-32-to-64.html)
- [Unity games](https://github.com/boggydigital/mac-gaming-guides/blob/main/common/unity-porting.md)

### <a id="RPG-maker-games"></a>RPG Maker games:
You can run RPG Maker 2000/2003 games with [EasyRPG Player](https://easyrpg.org/player/downloads/#release-macos). And another very recently created tool for running RPG Maker MV, MZ, XP, VX and VX Ace games is [RPG-Maker-MacOS-Launcher](https://github.com/m5kro/RPG-Maker-MacOS-Launcher). It isn't perfectly compatible with every game built on this software, especially if it uses Windows 32 API stuff, or additional Ruby scripts, but there are workarounds for a lot of issues. (thanks [u/HomeStarRunnerTron](https://www.reddit.com/user/HomeStarRunnerTron/)!)

### <a id="mac-source-ports"></a>Mac Source Ports

These are games or game engines which either became open source or were retro-engineered, compiled, signed, and notarized for macOS by Tom Kidd, supporting [156 games](https://www.macsourceports.com/) when this guide was published.

### <a id="scummvm"></a>ScummVM

[ScummVM](https://www.scummvm.org) is a port of various game engines that supports 381 different games when this guide was published.

### <a id="nintendo-64-recompilations"></a>Nintendo 64 Recompilations

These tools will provide native recompilations, with many quality-of-life improvements, of Nintendo 64 games if you provide the original N64 ROM:

- [The Legend of Zelda: Ocarina of Time](https://www.shipofharkinian.com/fr)
- [The Legend of Zelda: Majora's Mask](https://github.com/HarbourMasters/2ship2harkinian/releases)
- [The Legend of Zelda: Majora's Mask](https://github.com/Zelda64Recomp/Zelda64Recomp): different static recompilation project from the above
- [StarFox 64](https://github.com/HarbourMasters/Starship?tab=readme-ov-file)
- [Super Mario 64](https://mameonmacs.blogspot.com/2024/11/super-mario-64-hacks-on-apple-silicon.html)
- [Perfect Dark](https://github.com/shinra-electric/Perfect-Dark-Build-Script)

### <a id="sonic-ports"></a>Sonic Ports

The first four are decompilations of [Christian Whitehead](https://en.wikipedia.org/wiki/Christian_Whitehead#Retro_Engine)'s various versions of the Retro Engine, the last one is a recompilation of the XBox 360 version of the game. For obvious copyright reasons, you must provide game files for these to work.

- [Sonic 1 & 2](https://github.com/Sappharad/Sonic-1-2-2013-Decompilation)
- [Sonic 3 & Knuckles](https://projects.sappharad.com/s3air_mac/)
- [Sonic CD](https://github.com/Sappharad/Sonic-CD-11-Decompilation)
- [Sonic Mania](https://github.com/Sappharad/Sonic-Mania-Decompilation)
- Sonic Unleashed [GitHub project](https://github.com/hedge-dev/UnleashedRecomp), [Apple Silicon binary download](https://github.com/squidbus/UnleashedRecomp/actions/runs/14414092340/artifacts/2931304077)

### <a id="individual-mac-ports"></a>Individual Mac Ports

- [Half-Life 2](https://github.com/Stoppedwumm/halflife2patcher) see also [this guide](https://jxhug.notion.site/Guide-to-Installing-Half-Life-2-Using-Source-Engine-on-macOS-9fa5ffc910f5454ab0f0e5da2a9e5b9f)
- [Half-Life 2 Episode 1 & 2](https://jxhug.notion.site/Guide-to-Installing-Half-Life-2-Using-Source-Engine-on-macOS-9fa5ffc910f5454ab0f0e5da2a9e5b9f)
- [Portal](https://jxhug.notion.site/Guide-to-Installing-Portal-Using-Source-Engine-on-macOS-660803f9ced149cfa1647d38fd5a7092)
- [Counter-Strike Source](https://jxhug.notion.site/Guide-to-Installing-Counter-Strike-Source-Using-the-Source-Engine-f956b3e067534cd0b9fed2ff7b21a64a)
- [Omori](https://github.com/SnowpMakes/omori-apple-silicon)
- [Asterix & Obelix XXL: Romastered](https://www.reddit.com/r/macgaming/comments/1im79hr/a_fix_for_asterix_obelix_xxl_romastered/) (note: while that makes the game run and play perfectly for the first level, the game crashes when starting level 2…)
- [Anime games](https://github.com/yaagl/yet-another-anime-game-launcher) (GI/HSR/ZZZ)
- [Final Fantasy XIV](https://www.xivmac.com/)
- [Badland: Game of The year edition](https://www.reddit.com/r/macgaming/comments/1j1daxy/a_fix_for_badland_game_of_the_year_edition/): fix for the crash at launch but the game still won't make any sound
- [FnMacAssistant](https://github.com/isacucho/FnMacAssistant): a bespoke utility to download and patch the iOS/iPadOS version of Fortnite and make it run on Apple Silicon-based Macs
- [AppleBlox](https://appleblox.com/): a Roblox launcher for macOS

## <a id="making-vr-work"></a>Making SteamVR work
People have made some progress with running tethered VR games:
- [video 1](https://www.youtube.com/watch?v=0tLuKZz15FY)
- [video 2](https://www.youtube.com/watch?v=Wzk3nBWMKL8)
(thanks [u/HomeStarRunnerTron](https://www.reddit.com/user/HomeStarRunnerTron/)!)
