# The macOS Game Workaround Repo
A centralized list of every known way to make games run on macOS

## Table of Contents

1. [Native Mac Games](#native-mac-games)
   - [Steam's Handling of 32-bit Mac Games](#steams-handling-of-32-bit-mac-games)
2. [Dual Booting](#dual-booting)
3. [iOS Versions](#ios-versions)
4. [Translation Layers](#translation-layers)
5. [Virtualization](#virtualization)
6. [Windows Games Available on Other Platforms](#windows-games-available-on-other-platforms)
   - [Multiple Platform Emulators](#multiple-platform-emulators)
   - [Sony PlayStation Series](#sony-playstation-series)
   - [Xbox Series](#xbox-series)
   - [Nintendo Consoles](#nintendo-consoles)
   - [Android](#android)
   - [MS-DOS Games](#ms-dos-games)
7. [Cloud Gaming](#cloud-gaming)
8. [Local Area Streaming](#local-area-streaming)
9. [Game Engine Ports, Hacks, and Patches](#game-engine-ports-hacks-and-patches)
   - [Unity and Adobe Air Games](#unity-and-adobe-air-games)
   - [Mac Source Ports](#mac-source-ports)
   - [ScummVM](#scummvm)
   - [Nintendo 64 Recompilations](#nintendo-64-recompilations)
   - [Individual Mac Ports](#individual-mac-ports)

## <a id="native-mac-games"></a>Native Mac Games

Native Mac ports are, most of the time, the best way to enjoy a game on your Mac. These can be purchased on the Mac App Store (some exclusively), some others are only available with an Apple Arcade subscription. The Mac App Store sometimes offers Universal Purchase, which means you can play the game on every supported Apple platform for a single fee. Other online stores offer Mac games, such as [GOG](https://www.gog.com/), the [Epic Games Store](https://store.epicgames.com/), and [Steam](https://store.steampowered.com/). Unlike the other stores, GOG provides games without DRM. The Epic Games Store is also notable for offering free games each Thursday, although Mac games are quite rare (many games that offer a Mac version are only available for Windows on the Epic Games Store). Note that the Epic Games Launcher will launch Universal Binary games as Intel processes on Apple Silicon Macs, you can launch games from the Epic Games Store as Apple Silicon processes with [Heroic Games Launcher](https://heroicgameslauncher.com/). The store with the largest game selection by far is Steam. When you buy a game on GOG, the Epic Games Store or Steam, you also get a license for the Windows version, and Linux version when available.

### <a id="steams-handling-of-32-bit-mac-games"></a>Steam's Handling of 32-bit Mac Games

When Apple decided to drop 32-bit support with macOS Catalina, many Mac games became non-functional overnight. The games could still be played provided you didn't update macOS but since Steam eventually also dropped support for 32-bit on macOS, there is no way to run these games natively on macOS anymore.

Valve decided to make developers manually specify if their game was 64-bit in the SteamWorks database, making games 32-bit by default. Games assumed to be 32-bit are signaled as incompatible with macOS Catalina and newer on the game's page. They are also signaled in the game library by an icon (🚫) next to their name and a message on their description page. These games are filtered out when clicking on the Apple logo to filter the Mac-only games. Unfortunately, many 64-bit games are incorrectly assumed to be 32-bit. Some of them are listed [here](https://docs.google.com/spreadsheets/d/17DkOsI9AwAT4dzPkLmunYJJmUpf1FuWR62Q1vAEfJzM/edit?usp=sharing).

Additionally, games that were delisted from the store are all reported as 64-bit in the library, whether the database says so or not.

## <a id="dual-booting"></a>Dual Booting

Intel-based Macs can boot on Windows like any other PC thanks to [Boot Camp](https://support.apple.com/us-en/guide/bootcamp-control-panel/bcmp29b8ac66/mac). This will run any Windows game. Apple Silicon-based Macs cannot boot under Windows, but while Apple said it was open to making Boot Camp available for Apple Silicon-based Macs to run Windows for Arm, they said it only depended on Microsoft's approval.

## <a id="ios-versions"></a>iOS Versions

Some Windows games have also been released for iPhone and iPad, and those apps can technically run on Apple Silicon-based Macs when allowed by their publishers on the App Store, which is arguably the closest thing to a native macOS version. However, many games are simply not allowed to run. There is a workaround for this with [PlayCover](https://playcover.io/) and [Sideloadly](https://sideloadly.io/), which are free apps that allow sideloading and running decrypted iOS/iPadOS apps on Apple Silicon-based Macs.

## <a id="translation-layers"></a>Translation Layers

These apps will translate Windows API calls to macOS API calls on the fly. While this solution doesn't make every Windows game work flawlessly (most notably, games with anti-cheat protection won't run), it's usually the best option to try first. These are all derived from Wine, and do not run Windows itself. There are several interfaces to translate various versions of [DirectX](https://en.wikipedia.org/wiki/DirectX) (Windows' graphical API) to [Metal](https://en.wikipedia.org/wiki/Metal_(API)) (macOS' graphical API), such as [DXVK](https://github.com/doitsujin/dxvk) (which translates DirectX 8/9/10/11 to Vulkan, which in turn is translated to Metal with [MoltenVK](https://github.com/KhronosGroup/MoltenVK)), [WineD3D](https://fdossena.com/?p=wined3d/index.frag) (DirectX 1-11 to OpenGL), or [DXMT](https://github.com/3Shain/dxmt) (DirectX 11 to Metal). In 2021, Apple released [Game Porting Toolkit](https://developer.apple.com/games/game-porting-toolkit/), which includes D3DMetal, an interface for Wine and its derivatives that translates DirectX 12 to Metal, bringing improved speed and compatibility to software layer translation.

- [CodeWeavers CrossOver](https://www.codeweavers.com/crossover/) (paid)
- [Whisky](https://getwhisky.app/) (free and open source)
- [Kegworks](https://github.com/Kegworks-App/Kegworks) (successor of Wineskin, turns Windows games into macOS apps, free and open source)
- [Porting Kit](https://www.portingkit.com/) (turns Windows games into macOS apps, free)
- [Wine](https://www.winehq.org/) (command-line interface only, free and open source)

Note that CrossOver usually offers the best compatibility and most up-to-date versions of Wine and the various DirectX translation interfaces. Codeweavers is also the biggest contributor to the Wine project, so supporting them helps everyone. CrossOver also comes with CrossTies, a list of game-specific fixes and workarounds for games that don't run out of the box through Wine, but starting with CrossOver 25 this will be phased out due to a better general compatibility. A common misconception about CrossOver is that the fee only allows you to use it for a year, whereas in reality you can use it forever, but you only get a year's worth of free updates. [CXPatcher](https://github.com/italomandara/CXPatcher) is a non-official tool that allows updating CrossOver's dependencies.

If you want to know if a specific game runs, there are compatibility lists:
- [for CrossOver](https://www.codeweavers.com/crossover/#compatibility)
- [for Whisky](https://docs.getwhisky.app/game-support/index.html)
- [for Porting Kit](https://www.portingkit.com/games)

Sometimes, the Windows version of specific store launchers (GOG Galaxy, Epic Games Launcher, Amazon Games) can get updates which breaks support with the above software translation layers. A workaround is to use [Heroic Games Launcher](https://heroicgameslauncher.com/), a native launcher for Windows and Mac games for these stores.

## <a id="virtualization"></a>Virtualization

Virtualization is different from emulation, in that it runs the native code on the host processor, when they share the same instruction set. The processor is shared between the two environments at once. For instance, a Nintendo Switch runs on an Arm processor, and thus can be virtualized on an Apple Silicon-based Mac. While there is an overhead compared to running a game on its original platform since the hardware is shared with macOS, it's generally faster than emulation as it converts one instruction set to the other. Unlike translation layers, these will run and require installing complete operating systems (like Windows or Linux), and unlike translation layers are usually more general-purpose than focused on gaming.

- [Parallels Desktop](https://www.parallels.com/) (the only Microsoft-sanctioned solution to virtualize Windows for Arm, paid)
- [VMWare Fusion](https://blogs.vmware.com/teamfusion/2024/05/fusion-pro-now-available-free-for-personal-use.html) (free for personal use)
- [Oracle VirtualBox](https://www.virtualbox.org/) (free)
- [UTM](https://apps.apple.com/us/app/utm-virtual-machines/id1538878817) (free)

## <a id="windows-games-available-on-other-platforms"></a>Windows Games Available on Other Platforms

These games can be run either through emulation or virtualization depending on the processor of the platform it runs on. There are countless emulators available for macOS, supporting from the most famous to the most obscure platforms, so this list won't be exhaustive and will only feature the most prominent emulators. Some Windows games were also released on these platforms (or inversely, some games first released on older platforms were eventually released on Steam through emulation), which provides an alternative, and sometimes superior, way of playing them on macOS. Some emulators provide improvements to how the original games ran, for instance by increasing their resolution, or even supporting bespoke HD graphical packs, effectively offering a "remastered" experience.

### <a id="multiple-platform-emulators"></a>Multiple Platform Emulators

- [OpenEmu](https://openemu.org/), a scarcely updated, very polished, Mac-only app that emulates many platforms
- [RetroArch](https://www.retroarch.com/), incidentally also available on [Steam](https://store.steampowered.com/app/1118310/RetroArch/) and the [Mac App Store](https://apps.apple.com/us/app/retroarch/id6499539433)
- [Ares](https://ares-emu.net/)

### <a id="sony-playstation-series"></a>Sony PlayStation Series

- [ShadPS4](https://shadps4.net/) : PlayStation 4
- [RPCS3](https://rpcs3.net/) : PlayStation 3 ([compatibility list](https://rpcs3.net/compatibility))
- [AetherSX2](https://aethersx2.net/download-aethersx2-for-macs/) (discontinued and Apple Silicon only) and [PCSX2](https://pcsx2.net/) ([compatibility list](https://pcsx2.net/compat)): PlayStation 2
- [DuckStation](https://www.duckstation.org/) : PS1
- [Vita3K](https://vita3k.org) : PS Vita
- [PPSSPP](https://www.ppsspp.org/) : PSP

### <a id="xbox-series"></a>Xbox Series

- [Xemu](https://xemu.app/): Xbox

### <a id="nintendo-consoles"></a>Nintendo Consoles

- [Ryujinx](https://github.com/Ryubing/Ryujinx/releases): Switch
- [Cemu](https://cemu.info/): Wii U
- [Dolphin](https://www.dolphin-emu.org/): GameCube & Wii
- [Citra](https://citra-emulator.com/download) Nintendo 3DS

### <a id="android"></a>Android

This is technically virtualization and not emulation since Android runs on Arm processors just like Apple Silicon-based Macs.

- [Bluestacks Air](https://www.bluestacks.com/mac) (free)
- [MumuPlayer](https://www.mumuplayer.com/mac/) (paid)

### <a id="ms-dos-games"></a>MS-DOS Games

- [Boxer](https://boxer.thec0de.com/) : turns MS-DOS games into macOS apps with an intuitive interface
- [DOSBox](https://www.dosbox.com/)
- [Dreamm](https://aarongiles.com/dreamm/) a bespoke emulator for LucasArts games and some other miscellaneous MS-DOS games

Additionnally, Intel-based Macs can dual-boot on [Batocera](https://batocera.org/), a Linux distribution centered around launching emulators and rom with a dedicated interface.

## <a id="cloud-gaming"></a>Cloud Gaming

Cloud gaming runs games on a remote PC and streams the video output to your Mac in real-time, while your Mac just displays the video stream and sends controller inputs to the server. It requires a good internet connection, but it can sometimes be the only solution for specific games. Note that most of these only support a limited selection of games and require monthly payments.

- [NVidia GeForce Now](https://www.nvidia.com/en-us/geforce-now/)
- [Xbox Cloud Gaming](https://www.xbox.com/en-US/cloud-gaming)
- [Boosteroid](https://boosteroid.com/)
- [Shadow](https://shadow.tech/)
- [Amazon Luna](https://luna.amazon.com) (works in Google Chrome only, free weekly games with Amazon Prime)
- [AntStream](https://www.antstream.com) (retro games)
- [Steam Link](https://apps.apple.com/us/app/steam-link/id1246969117) (Mac App Store link) : a selection of Steam games can be played online with friends with games they own and run on their PC.

## <a id="local-area-streaming"></a>Local Area Streaming

There are also local streaming solutions allowing you to play on your Mac a game running on another machine you own:

- [Steam Link](https://apps.apple.com/us/app/steam-link/id1246969117) (Mac App Store link) : a selection of Steam games can be run on your PC and played on your Mac. You can also play online with friends with games they own and run on their PC.
- [PS Remote](https://www.playstation.com/en-us/support/games/playstation-remote-play-on-pc-and-mac/): allows streaming games from your PS5 to your Mac

## <a id="game-engine-ports-hacks-and-patches"></a>Game Engine Ports, Hacks, and Patches

### <a id="unity-and-adobe-air-games"></a>Unity and Adobe Air Games

It is possible to make a selection of Windows and Mac 32-bit games made with Unity or Adobe Air run on recent Macs, on occasion even as Universal Binaries.

- [Adobe Air games](https://pluskz.blogspot.com/2023/04/adobe-air-32-to-64.html)
- [Unity games](https://github.com/boggydigital/mac-gaming-guides/blob/main/common/unity-porting.md)

### <a id="mac-source-ports"></a>Mac Source Ports

These are games or game engines which either became open source or were retro-engineered, compiled, signed, and notarized for macOS by Tom Kidd, supporting [156 games](https://www.macsourceports.com/) when this guide was published.

### <a id="scummvm"></a>ScummVM

[ScummVM](https://www.scummvm.org) is a port of various game engines that supports 381 different games when this guide was published.

### <a id="nintendo-64-recompilations"></a>Nintendo 64 Recompilations

These tools will provide native recompilations, with many quality-of-life improvements, of Nintendo 64 games if you provide the original N64 ROM:

- [The Legend of Zelda: Ocarina of Time](https://www.shipofharkinian.com/fr)
- [The Legend of Zelda: Majora's Mask](https://github.com/HarbourMasters/2ship2harkinian/releases)
- [StarFox 64](https://github.com/HarbourMasters/Starship?tab=readme-ov-file)
- [Super Mario 64](https://mameonmacs.blogspot.com/2024/11/super-mario-64-hacks-on-apple-silicon.html)

### <a id="individual-mac-ports"></a>Individual Mac Ports

- [Half-Life 2](https://github.com/Stoppedwumm/halflife2patcher) see also [this guide](https://jxhug.notion.site/Guide-to-Installing-Half-Life-2-Using-Source-Engine-on-macOS-9fa5ffc910f5454ab0f0e5da2a9e5b9f)
- [Half-Life 2 Episode 1 & 2](https://jxhug.notion.site/Guide-to-Installing-Half-Life-2-Episode-1-2-Using-the-Source-Engine-3a5075bb77364537a87656d358687d67)
- [Portal](https://jxhug.notion.site/Guide-to-Installing-Portal-Using-Source-Engine-on-macOS-660803f9ced149cfa1647d38fd5a7092)
- [Counter-Strike Source](https://jxhug.notion.site/Guide-to-Installing-Counter-Strike-Source-Using-the-Source-Engine-f956b3e067534cd0b9fed2ff7b21a64a)
- [Combat Master](https://www.youtube.com/watch?v=KqimNLGQBD0)
- [Omori](https://github.com/SnowpMakes/omori-apple-silicon)
- [Asterix & Obelix XXL: Romastered](https://www.reddit.com/r/macgaming/comments/1im79hr/a_fix_for_asterix_obelix_xxl_romastered/) (note: while that makes the game run and play perfectly for the first level, the game crashes when starting level 2…)
