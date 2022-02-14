# diwako_cbrn

Mission script to add an arcadey CBRN mechanic into an Arma mission. This script is not meant to be realistic, as breathing in 2 seconds of some bad smell air causing death is not fun on a gameplay level. However, you can tune down the max CBRN damage to get this effect if you really wish!\
CBRN threats are abstracted into 4 threat levels which are color coded by green, yellow, orange and red.

Threat level diagram, all threats level requirements stack:

1. Green: No CBRN gear need
2. Yellow: Gas mask needed
3. Orange: Supply of fresh air needed
4. Red: Full CBRN suit needed

This script comes with a custom gasmask overlay, breathing sounds and custom low oxygen warning sounds.

## Custom CBRN damage

This script features custom CBRN damage values, it is just a flat number and when you go over this number, you die. As CBRN damage takes quite some time to decrease hence why it is not simulated and CBRN damage stays until respawn.\
If a unit reaches 50% of the maximum CBRN damage it will be informed that from now on the CBRN damage will go up passively even if full CBRN protection is present. The unit needs to decontaminate at a small decon shower. Walk up to the shower, activate it via ACE interactions and stand in it until you receive a new message!\
Warning: Water is finite, make sure to turn off the shower again!

## Set up and configuration

1. Copy the scripts folder into your mission
2. Adapt your description.ext
   - if you do not have a description.ext, create it next to the mission.sqm in the base folder of your mission
   - Adapt classes CfgFunctions, CfgSounds, and RscTitle like the example description.ext
3. Edit `config.sqf` in `scripts\cbrn`

## Functions for mission makers

There is only one function that is public to use for mission makers. It is the function to create zones, recommended to do so via `initServer.sqf`.

```sqf
/*
 * Arguments:
 * 0: Center of zone, position array
 * 1: Threatlevel between 0 and 4.9, float
 * 2: Radius of full effect, float
 * 3: Radius of partial effect, float
 *
 * Returns trigger/zone object
 */
[getMarkerPos "marker_0", 1.5, 25, 25] call cbrn_fnc_createZone;
```

The returned object is a trigger. With it you can either just leave it as is, attach it to some object, or toggle the zone on or off (it is on by default).

To turn the zone off set the variable `cbrn_active` to false, as such:

```sqf
_trg setVariable ["cbrn_active", false, true];
```

**WARNING**: Deleting the trigger object is not supported. First disable the zone, wait a few seconds for the value to sync in mp and then delete the trigger if you really must!

## Requirements

CBA_A3 and ACE3

## Variables

There are several variables preset for Vanilla and ACE3 gear, those can be overwritten either directly in the files or in your own `postinit` function or `init.sqf` file

### cbrn_maxDamage

Integer how much damage can be absorbed before death.\
If 50% of damage is reached passive contamination starts that needs to be stopped via decontamination showers!\
default: 100

### cbrn_masks

Array of strings, warning: CaSeSeNsItiVe!\
Array of facewear/goggles which are to be considered gasmasks.\
Default: Array of all vanilla Gasmasks and some select mod ones

### cbrn_backpacks

Array of strings, warning: CaSeSeNsItiVe!\
Array of backpacks which are to be considered oxygen tanks.\
Default: Array containing only the self-contained oxygen tank

### cbrn_suits

Array of strings, warning: CaSeSeNsItiVe!\
Array of uniforms which are to be considered cbrn suits.\
Default: Array of all Vanilla CBRN suits

### cbrn_threatMeterItem

String of item name that should be considered as threat meter item.\
Default: "ACE_microDAGR"

### cbrn_maxOxygenTime

Float, value in seconds of how long one oxygen container should last.\
Default: 30 Minutes

### cbrn_allowPassiveDamage

Boolean value if the passive contamination should occur after the 50% damage threshold

### cbrn_deconWaterTime

Float, value in seconds. Maximum total runtime

### cbrn_healingRate

Float, rate how much of the exposure damage is healed each second. **Healing is stopped if value is 0 or below, or player is experiencing passive contamination!**

## Links

BI Thread: https://forums.bohemia.net/forums/topic/225668-cbrn-script/
