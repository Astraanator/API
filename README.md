# **Damage-Lib and IncomingDamage-Lib**



## ---- DamageLib API ----

### Usage:

Check Users common Folder if the Lib available and download if Lib does not exist
```
local function FileExists(path)
    local file = io.open(path)
    if file then
	io.close(file)
	return true
    end
    return false
end

if not FileExists(COMMON_PATH.."Damage_Lib.lua") then   
    DownloadInternalFileAsync('Damage_Lib.lua', COMMON_PATH, function(success)
	if success then
	    PrintChat("Pls reload")
	end
    end)
end

require "Damage_Lib"
```

Params:

> getdmg("spell", target, source, stage, level)

### [spell] 
```
"Q" or "W" or "E" or "R" or "QM" or "WM" or "EM"   
"QM"/"WM"/"EM" == Evolved or Modified spells e.g. Nidalee, Gnar or Elise spells in theier variant Forms
"AA" <-- calculate Autoattack damage
"IGNITE" <-- calculate Ignite damage
"SMITE" <-- calculate Smite damage
stage 1 = "Smite"
stage 2 = "Upgraded Smite 1"
stage 3 = "Upgraded Smite 2"
```		 
### [target]
	--> object

### [source] 
	--> object

### [stage]
	--> 1 or higher
		Check DamageLib[Table] if there more stages for your Champion-Spell you like return.

### [level]
	--> [Optional]
		You can return a desired higher 
		Spell-Level-Damage than your current level.
		

### stage explanation Ahri[Q]
```	  
["Ahri"] = {
{Slot = "Q", Stage = 1, DamageType = 2, Damage = function(source, target, level) return ({40, 65, 90, 115, 140})[level] + 0.35 * APDmg(source) end},
{Slot = "Q", Stage = 2, DamageType = 3, Damage = function(source, target, level) return ({40, 65, 90, 115, 140})[level] + 0.35 * APDmg(source) end},
}
```	  
	  
**You see Ahri Q have 2 Stages [Stage = 1 and Stage = 2]**
	  
> Explanation:
```
Ahri[Q]

getdmg("Q", target, myHero, 1)
--> Stage = 1 return the calculated magical dmg for the Orb way there

getdmg("Q", target, myHero, 2)
--> Stage = 2 return the calculated true Dmg for the Orb way back
```
```
function Pseudo_AhriQ_KS()
    local QDmg = getdmg("Q", target, myHero, 1) + getdmg("Q", target, myHero, 2)
    if QDmg >= target.health then
	............
    end
end
```


## ---- Incoming DamageLib API ----

### Usage:

Check Users common Folder if the Lib available and download if Lib does not exist
```
local function FileExists(path)
    local file = io.open(path)
    if file then
	io.close(file)
	return true
    end
    return false
end

if not FileExists(COMMON_PATH.."Incoming_Dmg_Lib.lua") then   
    DownloadInternalFileAsync('Incoming_Dmg_Lib.lua', COMMON_PATH, function(success)
	if success then
	    PrintChat("Pls reload")
	end
    end)
end

require "Incoming_Dmg_Lib"
```


Params:
> GetIncomingDmg(game_object, hittime, IncDmg-Input, Damage-Typ)

### [game_object] 						
--> Object where you want the incoming dmg return
### [hittime]
--> [optional] Time from when the Spell Dmg should be calculated (default value = 0.2) <
### [IncDmg-Input]
--> [optional] Add here your table with the units from which you want to get the Incoming Damages 
### [Damage-Typ]
--> [optional] If we only want to return a specific dmg type like physical, magical or true damage
--> input ("AD" or "AP" or "TRUE") 



> Explanation:
```
local IncDmg_Input = {
    Damages = {"minion", "hero", "turret"}  
}

>> With this table we return all incoming Dmgs from Minions, Turrets and Heroes
```
```
local function GetIncDmg()
    local IncDmg = GetIncomingDmg(myHero, 0.2, IncDmg_Input, "AD") --<--- We return only Incoming physical damage
    if IncDmg > 0 then
	local PercentHpAfterDmg = (myHero.health-IncDmg)/myHero.maxHealth*100
	if PercentHpAfterDmg <= 20 then     ---<--- (if your hp falls below 20% due to the inc.dmg)
	    PrintChat(tostring("Incoming-Dmg: "..IncDmg))
	end
    end
end
```
