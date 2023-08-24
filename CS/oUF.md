# oUF

```lua
--[[
name: string
	Seems to be an identifier of some sort. 
func: callback(self, unit)
	This is the callback that will be use by oUF to create a frame that is usable by WoW. Some attributes must have specific names
]]--
function oUF:RegisterStyle(name, func)

```

```lua
--[[
Looks like this must be called before oUF:Spawn
name: string
	Same identifier as the one registered in RegisterStyle
]]--
function oUF:SetActiveStyle(name)

```

```lua
--[[
Looks like we must call oUF:SetActiveStyle before this.

Note: This seems to be called only once, for the player.

Seems to be the main thing to call to have something show on the screen. Used to create a single unit frame and apply the currently active style to it.

unit: string
	The frame's unit. e.g. player, target, party1
overrideName:
	unique global name to use for the unit frame. Defaults to an auto-generated name based on the unit. 
]]--
function oUF:Spawn(unit, overrideName)

```

```lua
-- sort of a setter on object https://wowpedia.fandom.com/wiki/API_Frame_SetAttribute

	object:SetAttribute("unit", unit)

```
# Ruri

```lua
--[[
self: table
	Provided by oUF. Seems to contain some initialization done by oUF
unit: string
	standard wow identifier, player, target, party1, etc
]]--
local function CreateVPlayerStyle(self, unit)
```


# lua
Basic structure is a table. It's a mix of array and dictionnary.

In a for loop, access the dictionnary elements with `pairs`. Even the array part will come out and the key will be the index of the item.

```lua
local myDictionary = {
  ["Blue Player"] = "Ana",
  ["Gold Player"] = "Binh",
  ["Red Player"] = "Cate",
}

for key, value in pairs(myDictionary) do
  print(key .. " is " .. value)
end
```

Access the array elements with `ipairs` (i for index)

```lua
local everything = {
	["Blue Player"] = "Ana",
	["Gold Player"] = "Binh",
	["Red Player"] = "Cate",
	"Ali",
	"Ben",
	"Cammy",
}
print("looping with ipairs. Returning only the array part")
for i, name in ipairs(everything) do
	print("Array item #" .. i .. " is " .. name)
end

print("looping with pairs")
for key, value in pairs(everything) do
	print("key:" .. key .. ", value: " .. value)
end
```

Output
```
looping with ipairs. Returning only the array part
Array item #1 is Ali
Array item #2 is Ben
Array item #3 is Cammy
looping with pairs. 
key:1, value: Ali
key:2, value: Ben
key:3, value: Cammy
key:Gold Player, value: Binh
key:Blue Player, value: Ana
key:Red Player, value: Cate
```


# WoW API

This seems to be really important to have the game pay attention to a frame
https://wowpedia.fandom.com/wiki/SecureStateDriver#UnitWatch

## UnitWatch

RegisterUnitWatch(frame, asState)
UnregisterUnitWatch(frame, asState)

Creates or cancels a **UnitWatch**. Frames must inherit [SecureUnitButtonTemplate](https://wowpedia.fandom.com/wiki/SecureUnitButtonTemplate "SecureUnitButtonTemplate") and have an assigned [unitId](https://wowpedia.fandom.com/wiki/UnitId "UnitId") using [Frame:SetAttribute](https://wowpedia.fandom.com/wiki/API_Frame_SetAttribute "API Frame SetAttribute")("unit", "_unitId_")

frame

[Frame](https://wowpedia.fandom.com/wiki/UIOBJECT_Frame "UIOBJECT Frame") - The UnitWatch acts on this frame by toggling its visibility or setting its attributes

asState

Boolean - Controls the behaviour of a **UnitWatch** with the following results:

-   True causes the **UnitWatch** to securely call [Frame:SetAttribute](https://wowpedia.fandom.com/wiki/API_Frame_SetAttribute "API Frame SetAttribute")("state-unitexists", _result_) using the outcome of the macro conditonal's evaluation as _result_
-   False causes the **UnitWatch** to securely call [Region:Show](https://wowpedia.fandom.com/wiki/API_Region_Show "API Region Show")() when the macro conditional evaluates to "show" or [Region:Hide](https://wowpedia.fandom.com/wiki/API_Region_Hide "API Region Hide")() when it evaluates to "hide"

## Examples

Showing/hiding a unit frame
```lua
local frame = CreateFrame("Button", "MyParty1", UIParent, "SecureUnitButtonTemplate")
frame:SetAttribute("unit", "party1")
RegisterUnitWatch(frame)
    
frame:SetPoint("CENTER")
frame:SetSize(50, 50)
frame:SetBackdrop({ bgFile = "Interface\\BUTTONS\\WHITE8X8", tile = true, tileSize = 8 })
frame:SetBackdropColor(1, 0, 0)
```

The snippet above will display a clickable unit frame for the "party1" unit (consisting solely of a red square in the center of your screen) only when the unit exists.

https://wowpedia.fandom.com/wiki/UIOBJECT_Frame

**Region** (inherits from [ParentedObject](https://wowpedia.fandom.com/wiki/UIOBJECT_ParentedObject "UIOBJECT ParentedObject")) is an abstract widget type for anything that can occupy an area of the screen. As such, Frames, Textures and FontStrings are all various kinds of Region. Region provides most of the functions that support size, position and anchoring, including animation. It is a "real virtual" type; it cannot be instantiated, but objects can return true when asked if they are Regions.
https://wowpedia.fandom.com/wiki/UIOBJECT_Region