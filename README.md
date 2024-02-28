# Wine to rule them all !

## PLEASE DO NOT REPORT BUGS ENCOUNTERED WITH THIS AT WINEHQ OR VALVESOFTWARE, REPORT HERE INSTEAD !

Wine-tkg is a build-system aiming at easier custom wine builds creation.


# Quick how-to :

(for dependencies, see the [wiki page](https://github.com/Tk-Glitch/PKGBUILDS/wiki/wine-tkg-git) )

**Independently of the distro used, you'll want MinGW compiler to build recent wine as it fails to build more often than not without it these days.**


## Download the source :

 * Clone the repo (allows you to use `git pull` to get updates) :
```
git clone https://github.com/whrvt/wine-tkg-oglf-osu.git
```

## Configuration/customization :

If you want to customize the patches and features of your builds, you can find basic settings in [config/basic.cfg](https://github.com/whrvt/wine-tkg-oglf-osu/blob/master/config/basic.cfg) and advanced settings in [config/advanced.cfg](https://github.com/whrvt/wine-tkg-oglf-osu/blob/master/config/advanced.cfg).

You can also create an external configuration file that will contain all settings in a centralized way and survive repo updates. A sample file for this can be found [here](https://github.com/whrvt/wine-tkg-oglf-osu/blob/master/wine-tkg-profiles/sample-external-config.cfg). The default path for this file is `~/.config/frogminer/wine-tkg.cfg` and can be changed in [config/advanced.cfg](https://github.com/whrvt/wine-tkg-oglf-osu/blob/master/config/advanced.cfg) with the `_EXT_CONFIG_PATH` option.


## Building :

### For Arch (and other pacman/makepkg distros) :

 * From the base directory (where the PKGBUILD is located), run the following command in a terminal to start the building process :
```
makepkg -si
```
**This will install to `/opt/wine-osu`, as it is not recommended to use this as your main wine installation. Therefore, you must run all wine binaries directly from `/opt/wine-osu/bin/`**

### For other distros (make sure to check the [wiki page](https://github.com/Tk-Glitch/PKGBUILDS/wiki/wine-tkg-git)) :

**UNTESTED FOR THIS SPECIFIC FORK**

 * From the base directory (where the PKGBUILD is located), run the following command in a terminal to start the building process :
```
./non-makepkg-build.sh
```
**Your build will be found in the `PKGBUILD/wine-tkg-git/non-makepkg-builds` dir (independently of the chosen configuration)**

## Notes on built packages :

Possibly dependant on the compilation flags (found in [config/advanced.cfg](https://github.com/whrvt/wine-tkg-oglf-osu/blob/master/config/advanced.cfg)), the resulting wine binary may only work with a reduced subset of features in certain applications. It's recommended to only use this build for osu!.

For example: when running osu!stable under a WINEPREFIX which is missing `gdiplus` or `gdiplus_winxp`, wine's built-in version of gdiplus may crash when rendering certain areas of text, like the friends list or skins list. Furthermore, at least on some systems, `dotnet48` may not be usable, while `dotnet40` or `dotnet45` works fine.

On my system, this is an example of a fully working WINEPREFIX (to be used for playing osu! stable) created with [winetricks](https://github.com/Winetricks/winetricks):

```
WINEARCH=win64 WINEPREFIX=<PREFIX_DIRECTORY> WINE=/opt/wine-osu/bin/wine winetricks -q nocrashdialog dotnet48 cjkfonts meiryo gdiplus_winxp sound=pulse fontsmooth=rgb win2k3
```

WINEARCH=win32 is untested.

Some useful environment variables for the resulting wine binary can be found in [wine.install](https://github.com/whrvt/wine-tkg-oglf-osu/blob/master/wine.install).

Also, some error and warning messages can be silenced by overriding `winemenubuilder.exe` in winecfg to `disabled`.

## Credit :

A special thank you to [Torge Matthies (openglfreak)](https://github.com/openglfreak) for his hard work in compiling this large set of Wine patches. These are located in the `patches` directory.

The repo containing his patches, config, and renaming scripts can be found at https://github.com/openglfreak/wine-tkg-userpatches. They are are taken from the `tmp8` branch, for wine-staging 8.18.
