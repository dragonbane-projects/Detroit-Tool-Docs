README INVOKES/SHADERS

*General*

Important files:
- Invokes: DetroitBecomeHuman\Mods\DB0\_charInvokeFxPresets.lua
- Shaders: DetroitBecomeHuman\Mods\DB0\_charShaderAnimPresets.lua

These files can be edited in any text editor and are configuration files. You need to keep careful attention to the syntax and the placement of proper commas and brackets or you will get error messages as soon as you enter the menu. The error will tell you the line it found an error. You can check for errors in advance by copy and pasting the whole file into here which will tell you in a more descriptive way if there are errors: https://rextester.com/l/lua_online_compiler (NOTE: if you only get the error "attempt to index a nil value (global 'DB0')" that means the file is actually correct)

*Invokes*

Invokes are simple to configure. They are used to trigger special effects on characters and areas. Both invokes and shaders follow the same basic structure:

Example:
["Characters"] = {
	["Global"] = {
	},
	["1256"] = { --Markus Junkyard
        	["1. Repair Markus"] = {
            		delay = 0,
            		events = {
                		"R_LEG_SHOW",
                		"L_LEG_SHOW",
                		"CAPSULE_HIDE", --Pump. Show with CAPSULE_SHOW
                		"R_EYEBALL_HIDE", --Damaged Eyeball
                		"R_EYE_SHOW",
                		"R_EAR_SHOW",
                		"HEART_RED_TO_BLUE",
                		"Evt_SKIN_LED_OFF_TO_ON", --Skin retraction around LED
                		"Evt_LEDHOLE_CLOSING",
                		"Evt_SKIN_LEG_R_ON",     
                		"Evt_SKIN_LEG_L_OFF_TO_ON",
                		"Evt_SKIN_BODY_OFF_TO_ON",
                		"Evt_SKIN_BODY_ON",
                		"Evt_SKIN_HEAD_ON",
                		"Evt_SKIN_HEAD_OFF_TO_ON",
                		"Evt_SKIN_EAR_OFF_TO_ON",
                		"Evt_HEART_CASTSHAD_OFF"
            		}
        	},
	}
}

Global means that whatever name and effect you put here will appear for every character. Useful for global invokes that almost every character uses such as controlling the LEDs. 
Anything after you add into block sections with ID names is just for one catalog. So "1. Repair Markus" will only appear for characters that are currently using catalog ID 1256 which happens to be "From the Dead" Markus.
This keeps things tidy. You can copy and paste the Markus block for your own thing, just watch out for the proper placement of commas between blocks.

You can find catalog IDs using the reference file. Additionally you can select any character in the game itself in the "Character" menu and use the "Print Info" option which will print the used catalog ID to the "ID:" field. 

Invokes use the following parameters illustrated with the above Markus example:
delay = 0,
events = {
                "R_LEG_SHOW",
                "L_LEG_SHOW",
                "CAPSULE_HIDE", --Pump. Show with CAPSULE_SHOW
                "R_EYEBALL_HIDE", --Damaged Eyeball
                "R_EYE_SHOW",
                "R_EAR_SHOW",
                "HEART_RED_TO_BLUE",
                "Evt_SKIN_LED_OFF_TO_ON", --Skin retraction around LED
                "Evt_LEDHOLE_CLOSING",
                "Evt_SKIN_LEG_R_ON",     
                "Evt_SKIN_LEG_L_OFF_TO_ON",
                "Evt_SKIN_BODY_OFF_TO_ON",
                "Evt_SKIN_BODY_ON",
                "Evt_SKIN_HEAD_ON",
                "Evt_SKIN_HEAD_OFF_TO_ON",
                "Evt_SKIN_EAR_OFF_TO_ON",
                "Evt_HEART_CASTSHAD_OFF"
            }

Explanation:
This invoke repairs "From the Dead" Markus from his default broken appearance to his final form. The parameters mean the following:
- delay: Says how long to wait before applying all of the specified events (in seconds). 0 means the effects are applied instantly. A delay can sometimes be useful if you want to close the menu in time and record the effect happening on video
- events: Allows you to specify 1-x invokes to execute as a batch or just a single one. Super useful for this one in particular cause "From the Dead" Markus takes many events to fully fix him up

A list of possible Invokes per character/catalog can be found in the respective reference file under: "Useful Files/allInvokes.json".

NOTES:
- The effects will not be shown in the game menu in the order you have them in the file, but alphabetically for technical reasons. So that's why I personally choose to name them 1. xx, 2. xx, 3. xx cause 1-9 is alphabetical so it ensures they do appear like I have them in the file, which I prefer cause it's simpler to me
- A line starting or ending with "--" means that a comment is following after it. Comments can be whatever you want and are optional. They get ignored, but can be useful for annotations. "--Markus Junkyard" thus is a comment


*Shaders*

Shaders are a bit more advanced. They are used to control exposed parameters of character/area shaders to alter shader/rendering behavior. Please read the above "Invoke" section first as most of it still applies here and won't be repeated.
File structure is the same as it is for Invokes (global and per catalog entries).

Example:
["1. Trigger R Hand ON"] = {
            delay = 1,
            shaders = {
                "_FXD_CONNOR_RETRACTSKINHANDRIGHT_:CTRL_RETRACT_RIGHT_HAND"
            },
            startValue = 0,
            endValue = 1,
            blendDuration = 5,
            eventsOnStart = {
                "Evt_RETRACT_SKINHAND_RIGHT_ON"
            },
            eventsOnEnd = {
            }
        },

Explanation:
This shader controller turns Connor's right hand from human skin to Android skin. It will start as a fully human hand and turn into full Android hand in 5 seconds. The parameters mean the following:
- delay: The delay (in seconds) when shader control should begin. 0 is immediate, as is the case for invokes
- shaders: A list of shaders you want to control (at the same time)
- startValue: At which value to start the shader control. Many shaders only have a valid range from 0 (0%) to 1 (100%), but this can wary. The exact range can only be determined with trial and error
- endValue: At which value to stop the shader control
- blendDuration: How long you want the control to last. Value is in seconds. During the timespan specified here, the shader control will interpolate the shader value from startValue to endValue.
 If your startValue is lower than the endValue, this will increase the value upwards. If startValue is higher, it will decrease the value until it reaches endValue.
 Interpolation means the value is constantly linearly adjusted towards the destination causing a smooth change in numbers in-between. The speed and granularity being solely determined by how long or short the blendDuration value is
- eventsOnStart: Allows you to specify Invokes that should be executed BEFORE the shader gets controlled. This is useful cause for example Connor's Android hand requires 
 the use of the "Evt_RETRACT_SKINHAND_RIGHT_ON" invoke first, then you can control the shader after. So I combined this functionality so you dont need to separately do an invoke, then control shader
- eventsOnEnd: Same as "eventsOnStart", but this list gets executed at the very end of shader control instead

A list of controllable Shaders per character/catalog and the default shader value can be found in the respective reference file under: "Useful Files/allShaderControllers.json"

NOTES:
- If you want an exact shader value instantly and don't care about blending/transition, you can set start and endValue to the same value and set blendDuration to 0.
 For example Connor's serial number uses a shader controller and a transition is useless here, so you might use that here to just instantly set the value to "60" for example, so Connor gets the "60" serial number
- Transitions are mostly useful if you want to record the effect on video, such as Connor's hand slowly turning from human skin to Android skin or vice versa. If this is never desired, you can always set the same values and keep blendDuration at 0
