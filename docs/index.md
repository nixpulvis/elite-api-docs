## Introduction

Elite:Dangerous writes a network log file primarily to help when investigating problems.

Third-party tools developers have been reading some of the entries in the network log file, mainly in order to track the player's location.

There is a clear demand from players for third-party tools, and from tools developers for more information from the game and/or server api.

The new Player Journal provides a stream of information about gameplay events which can be used by tools developers to provide richer, more detailed tools to enhance the player experience. The data records written to this journal are much more high-level then that written to the network log.

A short example of a player journal file (_**out of date, some events may have additional data**_):


```
{"timestamp":"2016-06-10T14:31:00Z", "event":"FileHeader", "part":1, "gameversion":"2.2", "build":"r113684 " },
{"timestamp":"2016-06-10T14:32:03Z", "event":"LoadGame", "Commander":"HRC1", "Ship":"SideWinder", "ShipID":1, "GameMode":"Open", "Credits":600120, "Loan":0  }
{"timestamp":"2016-06-10T14:32:03Z", "event":"Rank", "Combat":0, "Trade":0, "Explore":1, "Empire":0, "Federation":0, "CQC":0 }
{"timestamp":"2016-06-10T14:32:03Z", "event":"Progress", "Combat":0, "Trade":0, "Explore":73, "Empire":0, "Federation":0, "CQC":0 }
{"timestamp":"2016-06-10T14:32:15Z", "event":"Location", "StarSystem":"Asellus 	Primus", "StarPos":[-23.938,40.875,-1.344] }
{"timestamp":"2016-06-10T14:32:16Z", "event":"Docked", "StationName":"Beagle 2 Landing", "StationType":"Coriolis" }
{"timestamp":"2016-06-10T14:32:38Z", "event":"RefuelAll", "Cost":12, "Amount":0.234493 }
{"timestamp":"2016-06-10T14:34:25Z", "event":"Undocked", "StationName":"Beagle 	2 Landing", "StationType":"Coriolis" }
{"timestamp":"2016-06-10T14:35:00Z", "event":"FSDJump", "StarSystem":"HIP 78085", "StarPos":[120.250,40.219,268.594], "JumpDist":36.034 }
{"timestamp":"2016-06-10T14:35:22Z", "event":"Scan", "BodyName":"HIP 78085 A", "StarType":"G" }
{"timestamp":"2016-06-10T14:36:10Z", "event":"FSDJump", "StarSystem":"Praea Euq NW-W b1-3", "StarPos":[120.719,34.188,271.750], "JumpDist":6.823 }
{"timestamp":"2016-06-10T14:36:42Z", "event":"Scan", "BodyName":"Praea 	Euq NW-W b1-3", "StarType":"M" }
{"timestamp":"2016-06-10T14:38:50Z", "event":"Scan", "BodyName":"Praea 	Euq NW-W b1-3 3", "Description":"Icy body with 	neon rich atmosphere and major water geysers volcanism" }
{"timestamp":"2016-06-10T14:39:08Z", "event":"Scan", "BodyName":"Praea 	Euq NW-W b1-3 3 a", "Description":"Tidally locked Icy body" }
{"timestamp":"2016-06-10T14:41:03Z", "event":"FSDJump", "StarSystem":"Asellus Primus", "StarPos":[-23.938,40.875,-1.344], "JumpDist":39.112 }
{"timestamp":"2016-06-10T14:41:26Z", "event":"SupercruiseExit", "StarSystem":"Asellus 	Primus", "Body":"Beagle 2 Landing" }
{"timestamp":"2016-06-10T14:41:29Z", "event":"Docked", "StationName":"Beagle 2 Landing", "StationType":"Coriolis" }
{"timestamp":"2016-06-10T14:41:58Z", "event":"SellExplorationData", "Systems":[ "HIP 78085", "Praea Euq NW-W b1-3" ], "Discovered":[ "HIP 78085 A", "Praea Euq 	NW-W b1-3", "Praea Euq NW-W b1-3 3 a", "Praea Euq NW-W b1-3 3" ], "BaseValue":10822, "Bonus":3959 }
```

### ChangeLog

**Version 28**

**Changes for v3.7 beta 2 (May 2020)**

new events added:

- CarrierJumpCancelled 
- CarrierNameChanged (with callsign, name, carrierid) 
- new Route.json file 


events with new data:

- CarrierJumpRequest - added Body (name) and BodyID 
- Bounty - include localised Target   
- FSDTarget - include StarClass 


bugs fixed:

- CarrierStats  - don't write a ReservePercent value when in debt 
- Status.json - flags fixed in SRV, and Lat/Lon in station 
- Loadout - fix cases where loadout data was mix of SLF and mothership 
- EngineerProgress - now written when state updates 
- RefuelAll, RepairAll - fix null strings 
- Docked - fix bug where docking at a FC doesn't sometimes pick up nearby station name 


**Version 27**

**Changes for v3.7 beta 1 (April 2020)**

- Added events relating to Fleet Carriers (§11) 
- New CargoTransfer event (§12.52) 
- Change to Repair event (§8.38) 
- Added some station services (see 'Docked' §4.2) 


**Version 26**

**Changes for v3.5 (September 2019)**

- New SAASignalsFound event with bio/geo signals on planets and hotspots in rings 
- SAAScanComplete: add SystemAddress 
- Scan: add StarSystem name and SystemAddress 
- FSDTarget: add RemainingJumpsInRoute 
- CodexEntry,Touchdown,Liftoff: add NearestDestination 
- StatusFlags: add flags fsdJump, srvHighBeam 
- ShipTargeted: add powerplay info 


**Bugs fixed:**

- Fixed code that strips newlines out of strings 
- Fixed bug that caused blank system names when selling exploration data 
- In the CrimeVictim event, the Offender field should now have added localisation, if relevant 
- Fixed description of FactionEffects in MissionCompleted event 


**Version 25**

**Changes for v3.4 (April 2019)**

- FSDJump event – now includes "Body" info about the arrival star 
- Don't write a spurious "FighterRebuilt" event after docking SLF back in the ship 
- "ApproachSettlement" now includes body info 
- The "Loadout" event: 
	- no longer includes spurious ammo stats for energy weapons 
	- now includes UnladenMass and FuelCapacity info 
	- now written when docking SRV back in mothership 
	- now includes CargoCapacity, and MaxJumpRange 
	- Module item names are now consistently lowercase 
- Status.json: 
	- include LegalState 
	- includes info on nearby planet, and 'AltFromAvgRad" flag 
- Scan: include a star's subclass 
- Location: include DistFromStarLS 
- Add Conflicts data in FSDJump and Location 
- Include Vehicle ID for SLF/SRV (LaunchFighter, LaunchSRV, FighterRebuilt, FighterDestroyed, DockSRV, DockFighter, SRVDestroyed, CrewLaunchFighter) 
- Add info in Scan event to show if the body was previously discovered or mapped 


**Version 24**

**In v3.3.03:**

- Change to SystemFaction and StationFaction info in Location, FSDJump, and Docked events (to avoid have multiple FactionState values in a Json record) 


**Bugs Fixed**

- Any CR/LF is now removed from localised strings 


**Version 23 – v3.3.02 (Jan 2019)**

- SquadronStartup event (§10.11) 
- ProspectedAsteroid event (§12.33) 
- ReservoirReplenished event (§12.38) 
- Include station info in Location event, if starting when docked 
- Status.json: more detail on fuel level (§13) 


**Bugs Fixed**

- Status.json: fix GuiFocus value when viewing Codex (§13) 
- CodexEntry: fix SystemAddress (§6.1) 


**Documentation Errata**

- CrimeVictim event (was in v3.3, but not previously documented) (§12.11) 
- Fix in EngineerCraft: (§8.13) note property name is BlueprintName not Blueprint (since v3.0) 
- MissionAccepted: (§8.21) note property LocalisedName (does not match automatically-localised strings) 


**Version 22 – in v3.3 (beta 4)**

- Fixed duplicate scan events 
- Fix some wing mission cargo reported as stolen 
- FSSSignalDiscovered: add USSType info (§6.6) 
- Add new FSSAllBodiesFound event (§6.4) 
- Scan events generated automatically when entering system now logged as "ScanType":"AutoScan" (§6.3) 
- Add new event MultiSellExplorationData (§6.10) 
- Faction info in Location/FSDJump: if it's the player's squadron faction, add SquadronFaction:true, and flags HappiestSystem:true or HomeSystem:true if relevant (§4.8) 


**in v3.3 (beta3)**

- FSSSignalDiscovered – if signal is a USS, add a ThreatLevel value; if signal is a station, add IsStation:true; add SystemAddress(§6.6) 
- Cargo – fix spurious extra events; add a flag to indicate vessel=Ship or Vessel=SRV (§3.1) 
- CodexEntry: add SystemAddress .(§6.1) 
- Status.json, Flags: add NightVision flag (bit28/0x10000000) (§13) 
- SendText: include a journal entry for text to squadron or system chat (§12.42) 
- ReceiveText: include a 'Channel' parameter to show if message was from squadron or system chat (§12.36) 
- SAAScanComplete: remove lists of names of discoverers and mappers (§6.13) 


**Version 21 – for v3.3 (beta 2)**

- Include info on Faction Happiness– in Location and FSDJump events (§4.8,§4.12) 
- Status flags indicate if in Analysis mode, and GUI focus shows if in Orrery, FSS, SAA or Codex view (§13) 
- Fixed bug in MissionAccepted and MissionCompleted for donation missions, where the "Donated" key was written twice, one with a string, once with an int (§8.21,§8.22) 


Errata

- Note Faction Active states do not have a Trend value 
- Note FID value in Commander(§3.3), LoadGame(§3.8), ClearSavedGame(§3.2), NewCommander(§3.7) events with unique player id number 
- Note Status.json (§13) includes fuel and cargo info 
- Fix capitalisation for BodyName in SAAScanComplete (§6.13) 


**Version 20 – for v3.3 (beta 1) **_(released 30th Oct 2018)_

- Multiple faction activestates – in Location and FSDJump events (§4.8,§4.12) 
- The first "Cargo" event written to the journal contains full inventory (§3.1) 
- Added "AsteroidCracked" event (§7.1) 
- Added "SAAScanComplete" (Surface Area Analysis) event (§6.13) 
- Added "CodexDiscovery" event (§6.1) 
- Added "FSSDiscoveryScan" and "FSSSignalScan" events (§6.4,§6.6) 
- Added several events for Squadrons(§10) 


**Version 19 – for v3.3 **_(preview released 20th Sept 2018)_

- Simplify the "Category" in MaterialTrade 
- Clarify meaning of bit 14 in status file: was called "under ship" but actually indicates when turret is retracted 
- ApproachSettlement now includes Latitude and Longitude 
- Note Bounty event is different for Skimmer bounty 
- Update description of StoredShips event with InTransit flag 
- Add ActiveFine info to Docked event 
- EngineerProgress event at startup with summary for all engineers currently known 
- Note a new ShipTargetted event is generated after using KillWarrantScanner, with updated bounty for target 
- Note the MissionRedirected mission name now has any trailing "_name" removed 
- Added MyReputation in faction list in FsdJump and Location events 
- Added "FSDTarget" event when selecting a starsystem to jump to 
- Added MissionID to cargo to indicate if it is mission-related: in Cargo, CollectCargo, EjectCargo events 
- In ship loadout, indicate if it is 'hot' 
- Cargo summary is now written to a separate file, and updated when data changes 
- Add "HullHealth" stat in the "Loadout" event 
- MissionCompleted now indicates correct destination after redirection 


**Version 18 – for v3.0.4** (27th March 2018)

**Version 17 – for v3.0.3 **(19th March 2018)

**Version 16 – for v3.0.2 **(5th March 2018)

**Version 15 – for v3.0 – beta3 **(6th Feb 2018)

**Version 14 – for v3.0 – beta1** (25/Jan/2018)

**Version 13 - In 2.4 Open beta **_(24th Aug 2017)_

**Version 12 - In 2.4 beta1 **_(17th Aug 2017)_

**Version 11**_published 26/Jun/2017_

**Version 10**_published 29/Mar/2017 (for v2.3 beta 5)_

**Version 9**_published 20/Feb/2017 (for v2.3 beta)_

**Version 8**_published 10/Jan/2017 (for v2.2.03)_

**Version 7**_published 15/Nov/2016 (for release 2.2.02)_

**Version 6**_published 26/Oct/2016 (for 2.2 public release)_

**Version 1** was published 20/July/2016