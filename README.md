# config_displays

A simple shell script that allows the user to create xrandr commands through dmenu.
It is currently in a fully working state but I plan to keep developing it further.

![Demo](config_displays_demo.gif)

The demo does not show resolutions changing as I cannot find a screen recorder that works across resolution changes but it works. Some debug information is left on in the demonstration to show the aspect ratio algorithm in play - this is turned off in the release.

## Dependencies

- dmenu
	- suckless-tools (ubuntu)
- xrandr
	- x11-xserver-utils (ubuntu)

## Install

Move config_displays to your desired location within $PATH (I use $HOME/bin/), ensure script has execute permissions (use `chmod +x config_displays` if not) and run the command through the terminal.

If you just want to test out the script then you can change directory to where it resides and run it with `./config_displays` without needing it in your $PATH.

Optionally, I bound one of my FN keys on my laptop to run the command to make the function more accessible. I achieved this by adding: 
`bindsym XF86Display exec --no-startup-id config_displays "Display Mode"` to my i3 config ($HOME/.config/i3/config), change to a key of your preference and it should work fine.

### Note:

Due to the use of the locate command to assign the resolutions file to a variable within the script, there's a high chance it won't locate it upon first use. This is because the locate command uses a database to locate files that is only updated on a daily basis.
If you have superuser privileges, you can update it manually by running `sudo updatedb` and the script will be able to work fine there-on.

## Features

- ~~Entirely self-contained within one script, with no need to create external files.~~ Only uses one additional file needed to confirm abnormal aspect ratios. Will be removed if I can find a more efficient solution. 
- Dynamically fetches outputs and resolutions, allowing for compatibility across multiple systems.
- Includes aspect ratio algorithm to figure out the ratio of any resolution, something that xrandr does not do.
- Only shows relevant resolutions on first pass, presenting the user with resolutions in their default aspect ratio.
- Provides positional options for non-primary displays and the ability to turn off those displays.
- An autoconfig option to use xrandr's recommended options for the current output.
- Isn't hindered by interlaced resolutions (e.g. 1920x1080i, as shown in the demo.) 

## Future plans

- Options to go back a menu.
- Show compatible resolutions; currently not all resolutions are actually valid, despite xrandr displaying them.
- Improve aspect ratio algorithm to work better with weird ratio resolutions (e.g. 1366x768 returns 683:384, as shown in demo.) without the need for an additional file to compare against.
- Find a way to filter out outputs xrandr shows but you don't have.
- Countdown for the autoconfig feature in case of broken display.
- Code optimisation.
- More stuff I haven't thought of yet.

## Questionable design choices

- Assigning config_displays_resolutions to a variable and using the locate command:
	- I made this decision as testing showed that when the script is ran through the use of a shortcut rather than through the command-line would result in the `getrelativeresolutions` function returning incorrect results. I tried many different ways to overcome this issue and this fix worked.

## Assumed F.A.Q's

### *"The program is showing outputs I don't have, why is this?"*

That is due to xrandr's output, not from my script and from what I can surmise, it gets those outputs from /sys/devices/pci0000:00/0000:00:02.0/drm/card0/card0\* or somewhere similar. Regardless, no resolutions will display under those outputs and the script will exit shortly after. A future plan is to devise a way to remove those phantom displays.

### *"The program is showing resolutions that doesn't work, why is that?"*

Again, that is also due to xrandr. I have tried to negate this by adding the aspect ratio function but further down the line I will try to remove resolutions that outright don't work. When I tried those resolutions on my machine, xrandr didn't do anything anyway.

### *"There's resolutions missing from your document, preventing them being shown to me, when will you fix that?"*

All resolutions provided have been sourced from [a Wikipedia article](https://en.wikipedia.org/wiki/List_of_common_resolutions), starting from 640x480 onwards as I doubt many people will be using anything smaller. You can append your own resolutions to the file as long as you follow the same format used within. I would appreciate it if you opened a pull request of your new version of the resolution file if you do so that it can be available for others also.

### *"I've noticed a bug or have some improvements to your code, what should I do?"*

You can always open an issue on the Issues tab, or if you have a fix, you may want to fork the project and propose a pull request of your fix.
All help is greatly appreciated.
