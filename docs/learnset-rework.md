# Kanto Level-Up Learnset Rework — Full Plan

Complete per-line spec for the learnset pass. Compares our base
(`src/data/pokemon/level_up_learnsets.h`, Gen 3 FRLG lists) against the
**Yellow Legacy** proposal and gives a Gen-3-adapted target for every line.

Goal: no dead-weight Pokémon. Every fully-evolved line gets a usable STAB by
~L20, a strong STAB by ~L34, one coverage move, and an identity move — with no
stretch longer than ~15 levels on a sub-50-power move.

**Legend:** ✅ done · **KEEP** (already fine) · **TIGHTEN** (pull STAB earlier /
add 1 coverage move) · **REWORK** (structural — usually a gutted stone/trade evo
or a line with no real STAB).

---

## Ground rules for porting Yellow Legacy

1. **YL is Gen 1 moves only.** Copy its *shape* (STAB level, coverage slot,
   identity move), not its move list. Swap in Gen 3 moves — see cheat-sheet.
2. **YL rebalances base stats; we don't.** Judge power level against *our* stats.
3. **Physical/Special is type-based here** (`IS_TYPE_PHYSICAL = type < MYSTERY`).
   Fire/Water/Grass/Electric/Psychic/Ice/Dragon/Dark = **Special**; rest =
   **Physical**. Physically-statted mons on a special STAB type (Gyarados,
   Aerodactyl, Kabutops, Flareon, Ponyta) need physical *non-STAB* coverage.
4. Ascending level order. Evolved mons repeat the pre-evo's low entries + `1:`
   grants for evolution moves.

### Gen 1 → Gen 3 swap cheat-sheet

| YL / Gen 1 | Use instead | Note |
|---|---|---|
| Sludge | **Sludge Bomb** (90) | every Poison final gets this ~L31 |
| Mega Drain | **Giga Drain** (buffed 75/10) | every Grass final ~L34 |
| Rock Throw spam | **Rock Slide** by level ~L30 | tutor-only in our base — huge gap |
| Night Shade (dmg) | **Shadow Ball** | unless a weak-attacker wants fixed dmg |
| Bubblebeam only | + **Water Pulse** ~L20, **Surf** (HM) | |
| Wing Attack only | + **Aerial Ace** ~L18, **Drill Peck** ~L26 | birds have no early Flying finisher |
| Thunderbolt missing | add ~L28–32, **Thunder** ~L44 | Raichu/Magneton/Electrode/Electabuzz |
| (no Dragon STAB) | **Dragon Claw** (TM02, 80) ~L32 | Dratini line |
| Body Slam / Double-Edge / Dig / Crunch / Brick Break | Gen 1/3 — use freely | physical coverage |

---

## GRASS

**Bulbasaur** — TIGHTEN
`1:Tackle, 1:Growl, 3:LeechSeed, 7:VineWhip, 10:PoisonPowder, 13:SleepPowder, 16:RazorLeaf, 21:GigaDrain, 27:SweetScent, 31:Growth, 37:SludgeBomb, 45:SolarBeam`
**Ivysaur** `+1:LeechSeed,VineWhip` · RazorLeaf 16, GigaDrain 23, Growth 33, SludgeBomb 40, SolarBeam 48
**Venusaur** `+1:GigaDrain` · RazorLeaf 16, GigaDrain 23, SludgeBomb 40, PetalDance 46, SolarBeam 52
_Giga Drain workhorse + Sludge Bomb for the Poison typing; drop Synthesis (weak in-game)._

**Oddish** — TIGHTEN
`1:Absorb, 5:SweetScent, 9:Acid, 12:PoisonPowder, 14:StunSpore, 16:SleepPowder, 20:MegaDrain, 26:GigaDrain, 32:Moonlight, 39:PetalDance, 44:SludgeBomb`
**Gloom** `+1s` · GigaDrain 28, Moonlight 35, PetalDance 44, SludgeBomb 40
**Vileplume** — REWORK `1:Absorb, 1:Aromatherapy, 1:StunSpore, 1:SleepPowder, 1:Acid, 1:MegaDrain, 28:SludgeBomb, 34:GigaDrain, 40:PetalDance, 46:SolarBeam`
_Vileplume was gutted (5 moves) — give it drain + Sludge Bomb + SolarBeam finish._

**Bellsprout** — TIGHTEN
`1:VineWhip, 1:Growth, 6:Wrap, 11:PoisonPowder, 14:SleepPowder, 17:StunSpore, 20:Acid, 24:RazorLeaf, 29:GigaDrain, 36:SweetScent, 43:SludgeBomb, 50:Slam`
**Weepinbell** `+1s` · RazorLeaf 24, GigaDrain 30, SludgeBomb 42, Slam 52
**Victreebel** — REWORK `1:Stockpile, 1:SpitUp, 1:Swallow, 1:VineWhip, 1:SleepPowder, 1:SweetScent, 1:RazorLeaf, 1:Acid, 30:SludgeBomb, 38:GigaDrain, 46:SolarBeam`
_Victreebel gutted (7 moves @1, nothing after)._

**Exeggcute** — TIGHTEN
`1:Barrage, 1:Uproar, 1:Hypnosis, 7:Reflect, 13:LeechSeed, 17:Confusion, 21:StunSpore, 25:PoisonPowder, 28:Psybeam, 32:SleepPowder, 38:GigaDrain, 43:Psychic, 48:SolarBeam`
**Exeggutor** — REWORK `1:Barrage, 1:Hypnosis, 1:Confusion, 1:SleepPowder, 1:StunSpore, 19:Stomp, 25:Psybeam, 31:EggBomb, 34:GigaDrain, 40:Psychic, 47:SolarBeam`
_Exeggutor gutted — didn't learn Psychic at all; its whole identity is Psychic + Grass._

**Tangela** — REWORK
`1:Constrict, 1:Ingrain, 4:SleepPowder, 8:Absorb, 12:VineWhip, 15:Growth, 19:PoisonPowder, 23:MegaDrain, 27:StunSpore, 31:RazorLeaf, 36:GigaDrain, 42:AncientPower, 48:Slam`
_Mega Drain 31 was its ceiling — add Razor Leaf + Giga Drain._

---

## FIRE  ✅ *(committed this session)*

Charmander line · Vulpix/Ninetales · Growlithe/Arcanine · Ponyta/Rapidash · Flareon
— all reworked. Magmar/Magby and Moltres KEEP.

---

## WATER

**Squirtle** — TIGHTEN
`1:Tackle, 4:TailWhip, 7:Bubble, 10:Withdraw, 13:WaterGun, 16:Bite, 20:RapidSpin, 25:WaterPulse, 31:Protect, 37:RainDance, 43:SkullBash, 50:HydroPump`
**Wartortle** `+1s` · WaterPulse 27, SkullBash 45, HydroPump 53
**Blastoise** `+1s` · WaterPulse 27, Crunch 40, SkullBash 50, HydroPump 60 · *Surf (HM) is the real STAB*
_Water Pulse fills the Bubble→HydroPump gap; Crunch coverage on Blastoise._

**Psyduck** — TIGHTEN
`1:WaterSport, 1:Scratch, 5:TailWhip, 10:Disable, 14:Confusion, 19:WaterPulse, 24:Screech, 30:PsychUp, 36:Psybeam, 43:Amnesia, 50:HydroPump`
**Golduck** `+1s` · WaterPulse 19, Psybeam 36, Psychic 44, Amnesia 40, HydroPump 46
_Nothing between Confusion 16 and HydroPump 50 — add Water Pulse + Psybeam/Psychic._

**Poliwag** — TIGHTEN
`1:Bubble, 7:Hypnosis, 12:WaterGun, 17:DoubleSlap, 22:WaterPulse, 28:RainDance, 33:BodySlam, 38:BellyDrum, 44:HydroPump`
**Poliwhirl** `+1s` · WaterPulse 22, BodySlam 33, BellyDrum 43, HydroPump 48
**Poliwrath** — REWORK `1:WaterGun, 1:Hypnosis, 1:DoubleSlap, 1:BubbleBeam, 1:Mist, 27:WaterPulse, 30:BrickBreak, 35:BodySlam, 40:Submission, 46:HydroPump, 52:MindReader`
_Thin, and no Fighting STAB by level — its 95/95 offense wants Brick Break._

**Tentacool** — TIGHTEN
`1:PoisonSting, 6:Supersonic, 12:Constrict, 17:Acid, 22:BubbleBeam, 27:WaterPulse, 32:SludgeBomb, 38:Barrier, 44:Screech, 50:HydroPump`
**Tentacruel** `+1s` · WaterPulse 27, SludgeBomb 33, Barrier 40, HydroPump 50 · *Surf*
_Sludge Bomb (great vs Grass) + Water Pulse bridge._

**Slowpoke** — TIGHTEN
`1:Curse, 1:Yawn, 1:Tackle, 6:Growl, 13:WaterGun, 17:Confusion, 20:WaterPulse, 24:Disable, 29:Headbutt, 36:Amnesia, 40:Psychic, 47:PsychUp`
**Slowbro** `+1s` · WaterPulse 20, Psychic 40, Amnesia 36 · *Surf*

**Seel** — TIGHTEN
`1:Headbutt, 5:Growl, 9:WaterGun, 16:IcyWind, 21:AuroraBeam, 26:Rest, 32:WaterPulse, 37:TakeDown, 41:IceBeam, 47:Safeguard`
**Dewgong** `1:SignalBeam` `+1s` · WaterPulse 32, IceBeam 40, SheerCold 50, Blizzard 55
_Ice Beam 51→40; Dewgong has no Water damage move by level — add Water Pulse._

**Shellder** — TIGHTEN
`1:Tackle, 1:Withdraw, 8:IcicleSpear, 13:Supersonic, 18:IcyWind, 23:AuroraBeam, 28:Protect, 33:WaterPulse, 38:Leer, 43:IceBeam, 50:Clamp`
**Cloyster** — REWORK `1:Withdraw, 1:Supersonic, 1:AuroraBeam, 1:Protect, 1:IcyWind, 1:IceBeam, 1:SpikeCannon, 30:Spikes, 36:WaterPulse, 44:Clamp, 52:Blizzard` · *Surf*
_Cloyster gutted — a premier special sweeper with no Ice Beam and no Water move._

**Krabby** — TIGHTEN
`1:Bubble, 5:Leer, 9:ViceGrip, 13:Harden, 17:MetalClaw, 21:MudShot, 25:Stomp, 29:WaterPulse, 33:Crabhammer, 38:Protect, 44:Guillotine, 50:Flail`
**Kingler** `1:MetalClaw` `+1s` · MudShot 21, WaterPulse 29, Crabhammer 35, Guillotine 44, Flail 52
_Crabhammer 45/**57** → 33/35 — it's Kingler's only real STAB._

**Horsea** — TIGHTEN
`1:Bubble, 6:SmokeScreen, 10:WaterGun, 14:Leer, 18:WaterPulse, 22:BubbleBeam, 26:DragonBreath, 30:Twister, 34:Agility, 40:DragonDance, 46:HydroPump`
**Seadra** `+1s` · WaterPulse 18, DragonBreath 26, Agility 34, DragonDance 40, HydroPump 48
_Dragon Dance 50/**62** → 40; Dragon Breath coverage._

**Goldeen** — TIGHTEN
`1:Peck, 1:TailWhip, 10:Supersonic, 14:HornAttack, 18:Waterfall, 24:FuryAttack, 30:Agility, 36:WaterPulse, 42:Megahorn, 48:HornDrill`
**Seaking** `+1s` · Waterfall 18, WaterPulse 36, Megahorn 44, HornDrill 50
_Waterfall (HM, physical) is its real STAB — pull to 18; Megahorn 57→42._

**Staryu** — TIGHTEN
`1:Tackle, 1:Harden, 6:WaterGun, 10:RapidSpin, 15:Recover, 19:Swift, 24:BubbleBeam, 28:WaterPulse, 33:Camouflage, 37:Psybeam, 42:LightScreen, 46:HydroPump`
**Starmie** — REWORK `1:WaterGun, 1:RapidSpin, 1:Recover, 1:Swift, 1:BubbleBeam, 1:Confusion, 33:Psychic, 38:WaterPulse, 44:LightScreen, 50:HydroPump` · *Ice Beam (TM)*
_Starmie gutted — needs Psychic + a real Water STAB curve._

**Magikarp** — KEEP
**Gyarados** — REWORK
`1:Thrash, 1:Bite, 20:Twister, 24:Waterfall, 28:DragonRage, 32:Leer, 36:Crunch, 40:HydroPump, 44:RainDance, 50:DragonDance, 55:HyperBeam`
_125 Atk physical — Waterfall (HM, physical) as real STAB at 24, Crunch coverage; keep DD nuke._

**Lapras** — KEEP · *(optional Surf)*

**Vaporeon** — TIGHTEN
`1:Tackle, 1:TailWhip, 1:HelpingHand, 1:WaterGun, 8:SandAttack, 16:WaterPulse, 23:QuickAttack, 28:AuroraBeam, 34:AcidArmor, 40:Haze, 46:IceBeam, 52:HydroPump`
_Water Pulse bridge, Ice Beam by level._

**Omanyte** — TIGHTEN
`1:Constrict, 1:Withdraw, 8:WaterGun, 13:Bite, 19:MudShot, 25:BubbleBeam, 31:AncientPower, 37:Protect, 40:WaterPulse, 46:Tickle, 52:HydroPump`
**Omastar** `+1s` · BubbleBeam 25, AncientPower 31, WaterPulse 40, SpikeCannon 40, HydroPump 46
_HydroPump **65**→46, AncientPower 55→31._

**Kabuto** — TIGHTEN
`1:Scratch, 1:Harden, 10:Absorb, 15:WaterGun, 19:Leer, 25:MudShot, 31:AncientPower, 37:Endure, 40:MegaDrain, 46:MetalSound, 52:RockSlide`
**Kabutops** — REWORK `1:FuryCutter` `+1s` · MudShot 25, AncientPower 31, Slash 37, **RockSlide 40**, MegaDrain 46
_115 Atk physical — Rock Slide *by level* (its Rock STAB was special-only Ancient Power); Slash + Rock Slide + Cut(HM)._

---

## ELECTRIC  *(priority #1 — Raichu/Magneton/Electrode/Electabuzz never learn Thunderbolt)*

**Pikachu** — TIGHTEN
`1:ThunderShock, 1:Growl, 5:TailWhip, 8:ThunderWave, 11:QuickAttack, 15:DoubleTeam, 18:Spark, 22:Slam, 26:Thunderbolt, 33:Agility, 40:Thunder, 46:LightScreen`
**Raichu** — REWORK `1:ThunderShock, 1:TailWhip, 1:QuickAttack, 1:ThunderWave, 1:DoubleTeam, 1:Spark, 1:Thunderbolt, 30:Agility, 38:Thunder, 45:LightScreen`
_Raichu's entire post-evo list was 4 moves — mirror Pikachu + Thunder._

**Magnemite** — REWORK
`1:MetalSound, 1:Tackle, 6:ThunderShock, 11:Supersonic, 16:SonicBoom, 20:ThunderWave, 25:Spark, 30:Thunderbolt, 36:LockOn, 40:Swift, 46:Screech, 52:ZapCannon`
**Magneton** `+1s` · Spark 25, **Thunderbolt 32**, TriAttack 40, LockOn 44, Thunder 50, ZapCannon 58
_No Thunderbolt by level — Spark 26 → Thunderbolt 32; ZapCannon (50% acc) kept as the Lock-On combo._

**Voltorb** — REWORK
`1:Charge, 1:Tackle, 8:Screech, 12:SonicBoom, 17:Spark, 23:Rollout, 28:Thunderbolt, 34:LightScreen, 40:Thunder, 46:SelfDestruct, 50:Explosion, 54:MirrorCoat`
**Electrode** `+1s` · Spark 17, **Thunderbolt 28**, Thunder 40, Explosion 50, MirrorCoat 54
_Fastest mon in Kanto with no STAB — Spark 21 then nothing._

**Electabuzz** — TIGHTEN
`1:QuickAttack, 1:Leer, 1:ThunderPunch, 9:ThunderPunch, 15:LightScreen, 20:ThunderShock, 25:Swift, 30:Thunderbolt, 36:Screech, 42:ThunderWave, 48:Thunder`
_Thunderbolt **47**→30._

**Jolteon** — KEEP *(user reworked)*
**Zapdos** — KEEP

---

## PSYCHIC

**Abra** — KEEP *(evolves at 16)*
**Kadabra** `+1s` — TIGHTEN · Psybeam 21, Recover 25, FutureSight 30, Psychic **33**, RolePlay 36, Trick 43
**Alakazam** `+1s` — TIGHTEN · Psybeam 21, Recover 25, FutureSight 30, Psychic **33**, CalmMind 38, Trick 45
_Just pull Psychic 36→33._

**Drowzee** — TIGHTEN
`1:Pound, 1:Hypnosis, 7:Disable, 11:Confusion, 16:Headbutt, 20:Psybeam, 25:PoisonGas, 29:Meditate, 33:Psychic, 38:PsychUp, 43:Swagger, 48:FutureSight`
**Hypno** `+1s` · Psybeam 20, Meditate 29, Psychic 33, FutureSight 48
_Add Psybeam bridge (Confusion 11 → Psychic 31 gap)._

**Mr. Mime** — TIGHTEN · pull Psychic 43→**36**, otherwise keep (big list).
**Jynx** — REWORK
`1:Pound, 1:Lick, 1:LovelyKiss, 1:PowderSnow, 9:DoubleSlap, 13:IcyWind, 18:Confusion, 23:IcePunch, 28:Psybeam, 35:Psychic, 40:IceBeam, 47:BodySlam, 52:Blizzard, 58:PerishSong`
_No Psychic by level; Blizzard at **67**. Add Psychic 35 / IceBeam 40 / Blizzard 52._

**Mewtwo** — KEEP *(caught L70)*
**Mew** — TIGHTEN · `1:Pound, 10:Transform, 15:Confusion, 20:Psychic, 30:Metronome, 40:AncientPower, 50:Barrier`
_Psychic 40→20._

---

## FIGHTING  *(healthiest type — Gen 3 lists already deliver)*

**Mankey / Primeape** — KEEP · *(optional BrickBreak ~26)*
**Machop / Machoke / Machamp** — KEEP
**Hitmonlee / Hitmonchan** — KEEP
**Poliwrath** — see Water (REWORK)

---

## GROUND

**Sandshrew** — TIGHTEN
`1:Scratch, 6:DefenseCurl, 11:SandAttack, 15:Dig, 20:PoisonSting, 23:Slash, 28:Swift, 33:RockSlide, 38:FurySwipes, 42:Earthquake, 48:SandTomb, 54:Sandstorm`
**Sandslash** `+1s` · Dig 15, Slash 24, RockSlide 33, Earthquake 42, Sandstorm 54
_No Earthquake by level — Dig bridge, Rock Slide + EQ._

**Diglett** — TIGHTEN
`1:SandAttack, 1:Scratch, 5:Growl, 9:Magnitude, 13:Dig, 19:Astonish, 21:MudSlap, 26:Slash, 32:Earthquake, 38:SandTomb, 45:Fissure`
**Dugtrio** `1:TriAttack` `+1s` · Dig 13, Slash 26, SandTomb 26, **Earthquake 34**, RockSlide 40, Fissure 55
_Earthquake 41/**51** → 32/34 + Rock Slide._

**Cubone** — KEEP
**Marowak** `+1s` — TIGHTEN · Bonemerang 25, RockSlide 34, **Earthquake 40**, BoneRush 41, DoubleEdge 46
_Add EQ + Rock Slide (Thick Club makes it a monster)._

**Rhyhorn** — TIGHTEN
`1:HornAttack, 1:TailWhip, 10:Stomp, 15:FuryAttack, 20:RockBlast, 25:ScaryFace, 30:TakeDown, 34:RockSlide, 40:Earthquake, 46:ScaryFace, 52:Megahorn, 58:HornDrill`
**Rhydon** `+1s` · RockBlast 20, **RockSlide 34**, **Earthquake 40**, Megahorn 50, HornDrill 58
_Earthquake 52/**58** → 40; Rock Slide by level._

---

## ROCK  *(priority #3 — Rock Slide is tutor-only in our base)*

**Geodude** — TIGHTEN
`1:Tackle, 1:DefenseCurl, 6:MudSport, 11:RockThrow, 16:Magnitude, 21:RockBlast, 26:SelfDestruct, 30:RockSlide, 34:Rollout, 38:Earthquake, 44:Explosion, 50:DoubleEdge`
**Graveler** `+1s` · RockBlast 21, **RockSlide 30**, Earthquake 40, Explosion 48, DoubleEdge 56
**Golem** `+1s` · RockBlast 21, **RockSlide 30**, Earthquake 40, Explosion 50
_Rock Slide by level (currently only from the one-shot tutor); EQ ~40._

**Onix** — REWORK
`1:Tackle, 1:Screech, 8:Bind, 12:RockThrow, 16:RockBlast, 20:Harden, 24:Rage, 28:DragonBreath, 32:RockSlide, 38:Sandstorm, 42:Earthquake, 46:IronTail, 52:SandTomb, 56:DoubleEdge`
_No Rock Slide / EQ by level — pure filler without them._

**Aerodactyl** — REWORK
`1:WingAttack, 1:Bite, 8:Agility, 14:AerialAce, 20:Supersonic, 26:RockSlide, 32:ScaryFace, 36:Crunch, 42:AncientPower, 48:TakeDown, 54:HyperBeam`
_Physical (105 Atk) — Rock Slide *by level*, Aerial Ace Flying STAB, Crunch coverage._

---

## BUG  *(offense carried by the Cut → Bug 80-BP change)*

**Caterpie / Metapod** — KEEP
**Butterfree** — REWORK
`1:Confusion, 10:Confusion, 13:PoisonPowder, 14:StunSpore, 15:SleepPowder, 18:Supersonic, 22:SilverWind, 26:Psybeam, 30:Whirlwind, 34:Gust, 38:Psychic, 44:Safeguard`
_No Psychic by level at all; Silver Wind at **47**. Powders + Compound Eyes stay the identity._

**Weedle / Kakuna** — KEEP
**Beedrill** — REWORK
`1:FuryAttack, 1:PoisonSting, 10:Twineedle, 15:FocusEnergy, 20:FuryAttack, 25:PinMissile, 30:SludgeBomb, 35:SwordsDance, 40:Agility, 45:Endeavor`
_Sludge Bomb (Poison STAB) + Swords Dance; Pin Missile (now 25/95). Cut(HM) = 80-BP Bug STAB._

**Paras** — TIGHTEN
`1:Scratch, 6:StunSpore, 11:PoisonPowder, 17:LeechLife, 22:Spore, 27:Slash, 33:GigaDrain, 40:Growth, 46:Aromatherapy`
**Parasect** `+1s` · LeechLife 17, Spore 24, Slash 30, GigaDrain 36, Growth 44
_Spore earlier; Giga Drain 43/51→36; Slash + Leech Life (now 45) physical._

**Venonat** — TIGHTEN
`1:Tackle, 1:Disable, 1:Foresight, 9:Supersonic, 15:Confusion, 19:PoisonPowder, 24:LeechLife, 28:StunSpore, 32:Psybeam, 36:SleepPowder, 41:Psychic, 46:SludgeBomb`
**Venomoth** `1:SilverWind` `+1s` · Confusion 15, SilverWind 26, Psybeam 32, Psychic 40, SludgeBomb 44
_Psychic 41/**52**→40; Sludge Bomb; Silver Wind earlier._

**Scyther** — KEEP-ish · pull SwordsDance 36→**31**; Cut(HM) is the Bug STAB now.
**Pinsir** — TIGHTEN
`1:ViceGrip, 1:FocusEnergy, 7:Bind, 13:SeismicToss, 19:Harden, 25:Revenge, 30:BrickBreak, 35:SwordsDance, 41:Submission, 46:Guillotine`
_Swords Dance **49**→35; SD + Cut(HM) + Brick Break + EQ(TM) is the set._

---

## POISON  *(every line tops out at Sludge 65 — give each final Sludge Bomb ~L31)*

**Nidoran♀** — TIGHTEN
`1:Growl, 1:Scratch, 7:TailWhip, 12:DoubleKick, 16:PoisonSting, 20:Bite, 24:HelpingHand, 30:FurySwipes, 36:Crunch, 42:Flatter`
**Nidorina** `+1s` · DoubleKick 12, Bite 22, Crunch 40
**Nidoqueen** — REWORK `1:Scratch, 1:TailWhip, 1:DoubleKick, 1:PoisonSting, 20:Sludge, 22:BodySlam, 30:Crunch, 36:SludgeBomb, 40:Earthquake, 46:Superpower`
_Nidoqueen thin (Body Slam 22, Superpower 43) — mirror Nidoking's quality._

**Nidoran♂ / Nidorino** — TIGHTEN (mirror ♀ structure) · **Nidoking** — KEEP *(user reworked)*

**Ekans** — TIGHTEN
`1:Wrap, 1:Leer, 8:PoisonSting, 13:Bite, 18:Glare, 24:Screech, 28:Crunch, 33:Acid, 38:SludgeBomb, 44:Haze`
**Arbok** `+1s` · Bite 13, Glare 18, Crunch 28, SludgeBomb 38, Haze 46
_Drop the Stockpile/Swallow/SpitUp trio; add Crunch + Sludge Bomb. Glare (para) is the identity._

**Zubat** — TIGHTEN
`1:LeechLife, 6:Astonish, 11:Supersonic, 16:Bite, 21:WingAttack, 24:AirCutter, 28:Crunch, 32:ConfuseRay, 36:MeanLook, 40:SludgeBomb, 46:PoisonFang, 50:Haze`
**Golbat** `+1s` · AirCutter 24, Crunch 30, ConfuseRay 32, SludgeBomb 38, PoisonFang 46
_(Crobat carries same + earlier if it's in your dex.)_

**Grimer** — TIGHTEN
`1:PoisonGas, 1:Pound, 4:Harden, 8:Disable, 13:Sludge, 19:Minimize, 25:Screech, 31:SludgeBomb, 37:AcidArmor, 44:BodySlam, 50:Memento`
**Muk** `+1s` · Sludge 13, SludgeBomb 31, AcidArmor 37, BodySlam 44, Memento 50
_Sludge Bomb 43/**47**→31._

**Koffing** — TIGHTEN
`1:PoisonGas, 1:Tackle, 9:Smog, 14:SelfDestruct, 19:Sludge, 24:Smokescreen, 30:SludgeBomb, 34:Haze, 40:Explosion, 45:DestinyBond, 49:Memento`
**Weezing** `+1s` · Sludge 19, **SludgeBomb 30**, Haze 34, Explosion 44, DestinyBond 51, Memento 58
_Weezing had **no Sludge Bomb at all** — only Sludge 21._

---

## NORMAL / FLYING

**Pidgey** — TIGHTEN
`1:Tackle, 5:SandAttack, 9:Gust, 13:QuickAttack, 17:AerialAce, 21:Whirlwind, 25:WingAttack, 31:FeatherDance, 37:Agility, 44:MirrorMove`
**Pidgeotto** `+1s` · AerialAce 17, WingAttack 27, Agility 40, MirrorMove 49
**Pidgeot** `+1s` · AerialAce 17, WingAttack 27, Agility 44, **SkyAttack 50**, MirrorMove 58
_Aerial Ace bridge; Sky Attack identity on Pidgeot (YL's call)._

**Rattata** — TIGHTEN
`1:Tackle, 1:TailWhip, 7:QuickAttack, 10:FocusEnergy, 13:HyperFang, 19:Bite, 24:Dig, 28:SuperFang, 34:Crunch, 41:Endeavor`
**Raticate** `+1s` · HyperFang 13, Bite 19, Dig 24, SuperFang 34, Crunch 38, Endeavor 50
_Dig + Crunch coverage (Guts + Hyper Fang identity kept)._

**Spearow** — TIGHTEN
`1:Peck, 1:Growl, 7:Leer, 11:FuryAttack, 16:AerialAce, 22:Pursuit, 26:DrillPeck, 32:MirrorMove, 39:Agility`
**Fearow** `+1s` · AerialAce 16, Pursuit 22, **DrillPeck 26**, Agility 40
_Drill Peck 37/**40** → 26._

**Meowth** — TIGHTEN
`1:Scratch, 1:Growl, 10:Bite, 14:FakeOut, 18:PayDay, 24:FaintAttack, 28:Slash, 34:Screech, 40:FurySwipes, 46:Crunch, 50:Swagger`
**Persian** `+1s` · FaintAttack 24, Slash 28, Crunch 40, Swagger 48
_Slash 40/**49** → 28 (Persian is fast, needs earlier power)._

**Jigglypuff** — KEEP-ish · pull BodySlam 34→**30**
**Wigglytuff** — REWORK `1:Sing, 1:Disable, 1:DefenseCurl, 1:DoubleSlap, 1:Pound, 1:Rollout, 24:BodySlam, 30:Rest, 34:HyperVoice, 39:Mimic, 44:DoubleEdge`
_Gutted (4 moves) — mirror Jigglypuff._

**Clefairy** — KEEP *(full list, Meteor Mash 45)*
**Clefable** — REWORK `1:Sing, 1:DoubleSlap, 1:Minimize, 1:Metronome, 1:Encore, 1:DefenseCurl, 1:BodySlam, 28:CosmicPower, 34:Moonlight, 40:LightScreen, 45:MeteorMash`
_Gutted (4 moves) — mirror Clefairy + Meteor Mash._

**Farfetch'd** — TIGHTEN
`1:Peck, 6:SandAttack, 11:Leer, 15:FuryAttack, 18:AerialAce, 22:KnockOff, 26:Slash, 30:SwordsDance, 36:Agility, 42:FalseSwipe`
_Slash **41**→26; SD 31→30; Aerial Ace added — SD + Slash is the whole gameplan._

**Doduo** — TIGHTEN
`1:Peck, 1:Growl, 9:QuickAttack, 13:FuryAttack, 18:Pursuit, 22:AerialAce, 26:DrillPeck, 30:Rage, 36:Uproar, 42:Agility, 48:DoubleEdge`
**Dodrio** `+1s` · AerialAce 22, **DrillPeck 28**, TriAttack 30, Uproar 38, Agility 47, DoubleEdge 55
_Drill Peck 37/**47** → 26/28._

**Lickitung** — TIGHTEN
`1:Lick, 5:Supersonic, 9:DefenseCurl, 13:KnockOff, 18:Stomp, 23:Rollout, 28:BodySlam, 33:Slam, 38:Screech, 44:Refresh, 50:DoubleEdge`
_No Body Slam currently — add at 28 (its 90 Atk + huge HP wants it)._

**Chansey** — KEEP · *(optional SeismicToss 24)*
**Kangaskhan** — TIGHTEN
`1:CometPunch, 1:Leer, 7:Bite, 13:TailWhip, 19:FakeOut, 25:MegaPunch, 31:BodySlam, 37:DizzyPunch, 43:Crunch, 49:Reversal`
_Body Slam 31, Crunch 43 — 95 Atk with no real STAB by level._

**Tauros** — TIGHTEN
`1:Tackle, 1:TailWhip, 4:Rage, 8:HornAttack, 13:ScaryFace, 18:Pursuit, 23:Stomp, 28:BodySlam, 34:Swagger, 40:TakeDown, 46:Thrash, 52:DoubleEdge`
_Nothing between Horn Attack 8 and Thrash 43 currently — Stomp 23, Body Slam 28._

**Ditto / Eevee** — KEEP
**Porygon** — TIGHTEN · TriAttack 36→**28**, Recover 20→18
**Snorlax** — KEEP-ish · BodySlam 33→**29**, add Crunch 41

---

## ICE  *(Dewgong/Cloyster/Jynx handled in Water & Psychic)*

**Articuno / Zapdos / Moltres** — KEEP *(caught L50 with their STAB; optional: pull the L49 STAB to ~45)*

---

## DRAGON

**Dratini** — REWORK
`1:Wrap, 1:Leer, 8:ThunderWave, 15:Twister, 21:DragonRage, 27:Slam, 32:DragonClaw, 38:Agility, 44:Safeguard, 50:Outrage, 57:HyperBeam`
**Dragonair** `+1s` · Twister 15, DragonRage 21, **DragonClaw 32**, Agility 38, Outrage 47, HyperBeam 56
**Dragonite** `+1s` · **DragonClaw 32**, AerialAce 40, Outrage 44, Safeguard 47, HyperBeam 61 · *Fly (HM)*
_No real Dragon STAB until Outrage **50** — Dragon Claw (TM02, 80 BP, no drawback) by ~32. Pairs with the growth-rate buff you already did._

---

## MEWTWO / MEW

**Mewtwo** — KEEP · **Mew** — see Psychic (Psychic → L20)

---

## Implementation order

1. **Electric** — Raichu, Magnemite/Magneton, Voltorb/Electrode, Electabuzz. 4 lines, huge impact, small diffs.
2. **Stone/trade evos** — Vileplume, Victreebel, Exeggutor, Cloyster, Starmie, Poliwrath, Wigglytuff, Clefable, Nidoqueen. Whole-line dead weight.
3. **Rock Slide by level** — Geodude line, Onix, Rhyhorn line, Aerodactyl, Kabutops, Omastar, Sandshrew, Marowak.
4. **Sludge Bomb ~L31** — every Poison final (Nidoqueen, Arbok, Golbat, Muk, Weezing, Venomoth, Beedrill, Tentacruel, Vileplume, Victreebel).
5. **Giga Drain ~L34** — every Grass final (Venusaur, Vileplume, Victreebel, Exeggutor, Tangela, Parasect).
6. **Buried finishers** — Drill Peck (Fearow/Dodrio), Earthquake (Dugtrio/Rhydon/Sandslash/Marowak), Crabhammer (Kingler), Ice Beam (Dewgong), Outrage/Dragon Claw (Dragonite), Hydro Pump (Omastar/Golduck).
7. **Physical coverage on special-STAB mons** — Gyarados, Aerodactyl, Kabutops, Tauros, Kangaskhan, Lickitung, Ponyta line ✅.
8. Everything tagged **TIGHTEN** — the water-bridge (Water Pulse), Aerial Ace on birds, Psybeam bridges.
