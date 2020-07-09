# NOTE:
This is an upstream fork of https://github.com/CappyT/g14control that has features and bug fixes that are not yet deemed stable for the official release, but that I have tested and are stable enough for my day-to-day use. There will be new EXEs bundled in the [releases](https://github.com/carverhaines/g14control/releases/latest) section between official releases by CappyT. For those that want the bleeding edge updates/testing branches.

## G14Control
## A simple tray app to control Asus Zephyrus G14 Power options

#### Background:
If you are a user like me, you hate using a bunch of different apps to control all the power saving options of your laptop (sometimes even hidden in the registry) and prefer a simple, handy, tray app utility. The focus of this application is just that!
It does combine all the option offered from other utilities into one, single, configurable TrayApp.

#### What does it do?
G14Control (you can even rename it) can control the current ASUS Power plan, Fan curve, Processor Boost Mode, Processor TDP, dGPU Activation and Screen refresh rate to your needs with a simple right click on the Windows taskbar. You can configure all the presets (and add new ones too) from the `config.yml` file.

#### Does it work?
Not yet fully. See TODO for information about that.

#### What about Linux?
While is possible to port this app to Linux, at the moment is engineered to work only on Windows. It will be ported to Linux someday as I see the need for it.

### Installation
Download the latest release zip from GitHub: https://github.com/CappyT/g14control/releases

Extract it to some permanent location such as C:\Users\[username]\G14Control

Edit the config.yml with text editor as needed (see configuring below)

### Configuring
All done in config.yaml within the root folder of the program. The program must be restarted for any changes to the config.yaml to take effect.

`app_name:` can be customized, this is what the hover text displays over the icon and the windows notification title

`start_on_boot` Set this to `true` or `false`. Note this will create a Windows Registry entry to enable starting on login. Must have files extracted to a permanent location as above. Note: This will popup an administrator UAC prompt the first time you login after each boot.

`default_starting_plan` set plan name you want on boot or on restart of the program

`default_ac_plan` This plan name will automatically enable when AC adapter plugged in (set both default_ac_plan and default_dc_plan to `null` to disable this feature)

`default_dc_plan` This plan name will automatically enable when on battery power (set both default_ac_plan and default_dc_plan to `null` to disable this feature)

`default_gaming_plan` Enable this if you want the program to auto-switch plans based on games (or any program really) running. It will automatically switch to the plan specified here when the program is launched, and automatically switch back to the previous plan once it has closed. Set to `null` to disable.
- WARNING: this may be more resource intensive as it polls running processes on your computer every 10 seconds. However I noticed little difference, and almost no score change on Heaven Benchmark (FPS +/- 2).

`default_gaming_plan_games` This will be a list of exe's that you want to detect. Please check the exact name of the exe. For example, Steam is SteamService.exe. Example list: ["7zFM.exe", "notepad++.exe", "SteamService.exe"]

##### Notes on using Auto Power Switching:
- Only available if `default_ac_plan` and `default_dc_plan` are set in config (set to `null` to disable)
- Manually changing your plan thru the icon menu will DISABLE auto power switching.
- To re-enable, click the "Re-Enable Auto Power Switching" option in the icon menu.


##### Configure plans
Under Plans, you can configure as many or few plans as you want. A plan includes:
```
- name:
    This is where you will enter the name you want to be displayed for that plan
  plan:
    Name of the ROG Armory plan you want it set on (`silent` or `windows` or `performance` or `turbo`)
  cpu_curve:
    An array of `temps_in_deg_C:fanspeed_percent` for custom fan curve such as "30c:0%,40c:0%,50c:0%,60c:0%,70c:34%,80c:51%,90c:61%,100c:61%". Otherwise use `null` for default
  gpu_curve:
    An array of `temps_in_deg_C:fanspeed_percent` for custom fan curve such as "30c:0%,40c:0%,50c:0%,60c:0%,70c:34%,80c:51%,90c:61%,100c:61%". Otherwise use `null` for default
  cpu_tdp:
    The tdp you want for the CPU expressed in mW, use `null` for default or numeric (45000 = 45W)
  boost:
    Whether you want the CPU to boost above it's 3.0Ghz base clock speed, `true` or `false`
  dgpu_enabled:
    Whether you want the dedicated NVIDIA GPU enabled (uses more power, need for graphics/games), `true`, `false`
  screen_hz:
    The refresh rate of the screen. Can be 60 (numeric) or 120 (numeric) (for supported models) or null for default refresh rate of your screen
```

The config.yaml has many examples of plans included by default. Modify at will.

### Important Note about ASUS ROG Armory Crate
The ASUS ROG Armory Crate program will automatically change plans on wake, AC unplug, AC plugin. This will override G14's plan setting. To fix please do one of the following:

- Best option at the moment: Uninstall Armory Crate and Armory Crate Service. This will completely resolve any conflicts.

- Alternative option, set `plan_change_delay` in config.yaml to something around `10`. This will delay changing the plan for 10 seconds, Armory crate will usually change the plan in the first few seconds after plugging/unplugging/waking up, this will change it back.
  - Note: this will only work if Auto Power Switch is enabled as above, and currently it will not change on wake, so Armory Crate may still change your plan in some instances. So still recommend uninstalling Armory Crate

### Downloads:
Check the release tab!


### How to build:
Make sure python 3 and pip are installed. Then (as admin, in the source folder) run:

`pip install -r requirements.txt`

`python setup.py build`

Then copy config.yml to the \build\exe.win-amd64-3.8\ directory, then rename & zip that folder for release!

### Disclaimer
Please note that this is an ALPHA version of this software, which is still undergoing testing and development. The software and all content found on GitHub related to it are provided “as is”. We do not give any warranties, whether express or implied, as to the safety, reliability, suitability or usability of the software or any of its content.

We will not be held liable for any loss, whether such loss is direct, indirect, special or consequential, suffered by any party as a result of their use of the software or content. Any use of the software or scripts provided here is done at the user’s own risk and the user will be solely responsible for any damage to any computer system or loss of data that results from such activities.

Should you encounter any bugs, glitches, lack of functionality or other problems with the software, please let us know in GitHub so we can look into addressing it.

### Contribute:
You are very free to contribute with your code. I kinda suck at coding so any help is appreciated. Just submit a pull request, I will merge it or discuss it as soon as possible.

### TODO:
- Automatic config generation
- ~~Dynamic Menu generation based on configured profiles~~ Implemented
- ~~atrofac commands integration~~ Implemented
- ~~ryzenadj command integration~~ Implemented
- ~~Parallel notification spawning (right now when notification is displayed the whole app locks until the notification disappears)~~ Kinda buggy, but better.
- Different options for AC/DC modes
- Windows power plan switching (is it needed?)
- Logging
- Better binary storage
- Better UI (?)
- Better code comments
- Better ~~engrish~~ english (sorry, is just not my native language)
- .... you tell us!

### Contributors:
- https://github.com/FlyGoat/RyzenAdj
- https://github.com/cronosun/atrofac
- https://github.com/dedo1911
- https://github.com/carverhaines
- https://tools.taubenkorb.at/change-screen-resolution/
