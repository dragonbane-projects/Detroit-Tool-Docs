# AddOn System

AddOns are a convenient way to store, manage and distribute Detroit Mods without touching the main game files at all. This ensures seamless compatibility between Epic and Steam, reduces Mod size considerably and skips ever needing to repair the game files.

After installing the system, the AddOn folder can be found at the following location: Detroit\Mods\DB0\AddOn. AddOns can be used both to add or modify existing game content, e.g. deleted content restorations, texture mods, etc. In fact most of my Mods are delivered as AddOns for convenience. This also includes all the deleted content restorations.

Inside the AddOn folder you also find the AddOnsManager app. This app can be used to manage your **active** set of AddOns. Active means that these AddOns are currently in-use. You can in fact have multiple conflicting AddOns installed at the same time, as long as you only have one of them enabled. 

After launching the AddOnsManager tool make sure the Detroit Game Path is set correctly. The tool will auto discover all available AddOns and present them in the list. You can toggle them on and off here. Don't forget to hit Save after! Via the right click context menu you can also quickly enable or disable all of them.

The ***Test For Conflicts*** button can be used to tell you if any of the AddOns you currenly have enabled are causing a conflict with each other. This situation will occur if you enable multiple AddOns targeting the same content e.g. two AddOns both modding the same character even if each changes a different texture. Once this situation occurs it will effectively be random which AddOn "wins out" and ends up getting used while the other won't take effect at all.

>[!NOTE]
Switching between active AddOns **always** requires a full game reboot. I advise not to manipulate the AddOn folder in any way while the game is actively running. Finally savefiles created using one set of AddOns might not play nice with saves using another set.