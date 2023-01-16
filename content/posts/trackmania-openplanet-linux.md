+++
title = "Trackmania with Openplanet on Steam Deck/Linux"
author = ["Nick Guz"]
date = 2023-01-16T13:15:00-05:00
tags = ["linux", "steam-deck"]
categories = ["category1"]
draft = false
+++

## Base Game Installation {#base-game-installation}

Download the Ubisoft Connect installer to your Downloads directory

```shell
curl -o $HOME/Downloads/UbisoftConnectInstaller.exe -L https://ubi.li/4vxt9
```

Now we want to add the installer to steam:

1.  Click "Add a Game", then "Add a Non-Steam game" in the bottom left of your
    steam library.
2.  Click "Browse" and find the installer we just downloaded. You will probably
    need to change the "Look in" directory at the top to something like
    /home/username in the dropdown. Also change file type to "All Files" so you
    can actually see the .exe file.
3.  Open the file, then click "Add Selected Programs".
4.  Find UbisoftConnectInstaller.exe in your library now, right click it and go
    to properties.
5.  Click "Compatibility", check "Force the use of a specific Steam Play
    compatibility tool, then pick "Proton Experimental" from the dropdown.  Note:
    this may break in the future, but at the current moment proton experimental
    works well for this. If it doesn't work, try using an older version of
    proton.

Now you can just click "Play" and you should see the installer popup. Just run
through this like usual, but don't check the box at the end to start the
launcher just yet.

Once this is done, we want to change this library entry to open the launcher we
just installed instead of the installer.

1.  Go back into the ubisoft launcher properties in steam.
2.  Click "Browse" under "Target" and "Start In" and select "UbisoftConnect.exe"
    from the wine prefix we just created.  This should be the most recently
    updated prefix in `/home/deck/.local/share/Steam/steamapps/compatdata`.  The
    full path will look something like
    ```shell
    /home/nickguz/.steam/steam/steamapps/compatdata/2965145486/pfx/drive_c/Program Files (x86)/Ubisoft/Ubisoft Game Launcher/UbisoftConnect.exe
    ```

"Play" this again and it'll open the Ubisoft launcher. Sign in and download
Trackmania like usual.


## Openplanet Installation {#openplanet-installation}

1.  Download the Openplanet installer from <https://openplanet.dev/download>. Leave
    it in your Downloads directory.
2.  We want to run the installer with the same proton version and wine prefix
    that we just used.  Assuming that your most recently updated directory is the
    one we've been working with, you should be able to run these commands, maybe
    modifying the steam directory location:
    ```shell
    # Find the most recently modified prefix
    tm_pfx=$(ls -At $HOME/.local/share/Steam/steamapps/compatdata | head -1)

    # Set the Proton directory to use. If you didn't use experimental, you'll need to choose whichever version you did use
    proton_dir="$HOME/.local/share/Steam/steamapps/common/Proton - Experimental"

    # Now run the installer. You might need to change the Openplanet filename if a newer version has been released
    WINEPREFIX=$HOME/.local/share/Steam/steamapps/compatdata/$tm_pfx/pfx $proton_dir/files/bin/wine $HOME/Downloads/OpenplanetNext_1.25.21.exe
    ```
3.  If everything was set properly in the last step, you should be able to run
    through the installer like normal.  You should be able to find Trackmania in
    the C directory. It will be in `C:\Program Files (x86)\ Ubisoft\ Ubisoft Game
          Launcher\Games\Trackmania`.  Select this directory. If you don't see that
    there, the wine prefix is most likely not set correctly. Make sure it's set
    to the pfx dir of the correct prefix.

Okay, we're almost done. Now we just need to enable the `dinput8.dll`.

1.  Install protontricks if you don't already have it. On steam deck, this can be
    done through Discover.
2.  Open a terminal (can use Konsole on steam deck) and run this command to open
    winecfg on our prefix:
    ```shell
    protontricks -c winecfg $(ls -At $HOME/.local/share/Steam/steamapps/compatdata | head -1)
    ```
3.  Click the Libaries tab. In the "New override for library" box, enter
    `dinput8` and click Add, Apply, OK.

If all went well, the next time you boot up Trackmania you should see in the
topleft corner the little Openplanet notification. You probably want to edit
your controller layout if playing on steam deck and bind some button to F3 so
that you can actually open Openplanet.

And one last thing. If you want your Ubisoft Connect in steam to open Trackmania
directly, just add `uplay://launch/5595/0` to launch options. This will leave
the Ubisoft launcher open though after exiting, so just close that after closing
Trackmania, or just force close it through steam (B to abort on steam deck after
exiting Trackmania).

All done!
