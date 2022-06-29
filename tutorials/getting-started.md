Getting Started
============
Each LEGO Game made by TTGames is ever so slightly different from each other but the one thing that remains consistent with all of them is the use of archive files. These archive files are used to contain all of the game's assets (sometimes with compression) so the game can easily access them without the constraints of the Windows file system.
 
This tutorial covers getting your game "mod ready". To get your game ready for modding you need to extract the contents of the archive files and, depending on the game, you may need to patch the executable file.
 
## Prerequisites
To extract the archive files, you will need an application known as [QuickBMS](https://aluigi.altervista.org/quickbms.htm) alongside the scripts that make it work [script1](https://aluigi.altervista.org/bms/ttgames.bms). Depending on the game, you may need to patch the executable file using [TtGamesPatcher](https://github.com/AlubJ/TTGamesPatcher). More details about the games that need patching are [here](linkyhere).
 
>`Note:` [DATOneArchiver](https://github.com/IsaMorphic/DATOneArchiver) is available however it only supports the LEGO Games before LEGO Star Wars The Complete Saga.
 
## Preparation
Before you extract your game make sure you have enough space to extract the contents out, otherwise the extraction may fail.

We recommend you keep a modded version of your game elsewhere on your system.
 
## Extraction
To begin, drag and drop the `script.bms` file onto the `quickbms.exe` executable, you may need to use `quickbms_4gb_files.exe` if `quickbms.exe` fails to work. A file dialog should open up asking you to choose the archive files, selected all the `*.DAT` files you would like to extract, including DLC if you would like.

>`Note:` For Lego Jurassic World, Lego Star Wars: The Force Awakens, and Lego Dimensions, a slightly modified script is necessary, and some files may not extract correctly.
 
![Open DAT Files](https://i.imgur.com/yGLKYbL.png)
 
After opening the `*.DAT` files you will be prompted to select an extraction location. It is recommended that you extract the game files elsewhere from the source `*.DAT` file location. Ignore `unins000.dat` if present.
 
![Save Files](https://i.imgur.com/PXrbsfM.png)
 
While QuickBMS works its magic, you may see this prompt.
 
![Duplicate File](https://i.imgur.com/8dLvCgh.png)
 
It's recommended to overwrite all the files without asking, because `*.DAT` files almost always have multiple files stored in them.

## After Extraction
After the game has been extracted, we recommend that you copy all files except the `*.DAT` files into the new location with the extracted game files. Doing this will keep a modded version of your game separate from the main, unmodded version.

## Patching Executables
Newer games require a modified executable in order to run with the DAT files extracted, a list is as follows:
 - Lego Marvel's Avengers
 - Lego Star Wars: The Force Awakens
 - Lego Worlds
 - The Lego Ninjago Movie Video Game
 - Lego Marvel Super Heroes 2
 - Lego The Incredibles
 - Lego DC Super-Villains
 - The Lego Movie 2 Videogame

## Summary
Extracting the game files is needed to mod the game, whether you want to create your own mods or you just want to install existing mods for the game. This tutorial should be universal to all LEGO Games because the BMS scripts were made with that in mind.
