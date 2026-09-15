# Known Vanilla Bugs (FireRed Rev 1)

Tracking file for vanilla `pokefirered_rev1.gba` bugs found in the decomp source, worked
through one at a time. See memory `pokedex-learnset-page` / `build-setup` for related project
context.

**Approach:** the `BUGFIX` macro (`include/config.h`) is only force-defined when `MODERN=1`,
which this machine's build can't use (see [build-setup.md](build-setup.md) in project memory —
agbcc path only). So every fix below that already exists in source, gated behind
`#ifdef BUGFIX`, currently compiles as the buggy vanilla path. We are **not** flipping the
global flag — each bug gets scoped to its own always-on define and is verified individually
before moving to the next, so we can review behavior/memory impact one change at a time
(ROM is near-full on EWRAM/IWRAM).

**Status legend:** ⬜ not started · 🔄 in progress · ✅ fixed & verified · ⏭️ skipped (reason noted)

## Section A — fix already written, gated behind `#ifdef BUGFIX`

These just need their guard scoped to always-on for that one bug, then verification.

| # | Status | Location | Bug |
|---|--------|----------|-----|
| A1 | ✅ | [pokemon.c:3643](src/pokemon.c#L3643) | `SetMonData(MON_DATA_IVS, ...)` reads only 1 byte (`*data`) instead of the full 4-byte IV bitfield — only `hpIV` and the low 3 bits of `attackIV` get set; `defenseIV`/`speedIV`/`spAttackIV`/`spDefenseIV` become 0. Fixed: collapsed `#ifdef BUGFIX` to the corrected 4-byte read, build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A2 | ✅ | [battle_main.c:3923](src/battle_main.c#L3923) | Roamer outcome check uses `&` instead of `==`; using Roar against a roamer (outcome = `B_OUTCOME_PLAYER_TELEPORTED`) incorrectly deactivates it as if defeated/caught. Fixed: collapsed to `==` check, build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A3 | ✅ | [battle_script_commands.c:3331](src/battle_script_commands.c#L3331) | Double-battle EXP stat copy for slot 2: Speed copied twice, Special Defense never copied — stale SpDef after mid-battle EXP gain. Fixed: replaced duplicate Speed line with the SpDef copy, build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A4 | ✅ | [battle_script_commands.c:5963](src/battle_script_commands.c#L5963) | `Cmd_recordlastability` advances the battle-script cursor by 1 byte instead of the command's actual 2-byte size, misaligning script parsing. Fixed: collapsed to the `+= 2` advance, build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A5 | ✅ | [berry_crush.c:1748](src/berry_crush.c#L1748) | Berry Crush sparkle-amount calc reads/writes the wrong struct field twice — only sparkle levels 0, 1, or 4 are ever attainable (2 and 3 are skipped). Fixed: removed the `field` macro indirection, both middle branches now write `sparkleAmount` directly. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A6 | ✅ | [data/pokemon/trainer_class_lookups.h:150](src/data/pokemon/trainer_class_lookups.h#L150) | Battle Frontier facility trainer pics: Elite Four Agatha/Lance slots show Lorelei/Bruno's sprites instead of their own. Fixed: both entries now point at their own `TRAINER_PIC_ELITE_FOUR_*` constants (verified those constants are used correctly elsewhere in the codebase). Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A7 | ✅ | [dodrio_berry_picking.c:2255](src/dodrio_berry_picking.c#L2255) | "All players ready" loop counter isn't reset before the check loop runs, so the loop never executes — minigame can start before all players are ready. Fixed: `i = 1;` reset unconditionally before the check loop. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game (multiplayer feature, hard to verify solo). |
| A8 | ✅ | [dodrio_berry_picking.c:3691](src/dodrio_berry_picking.c#L3691) | Per-player Dodrio sprite ID allocation is never freed (`FREE_AND_SET_NULL` skipped) — memory leak each time the minigame ends. Fixed: `FREE_AND_SET_NULL(sDodrioSpriteIds[i])` now runs unconditionally; confirmed the backing alloc via `AllocZeroed(4)` at line 3586. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A9 | ✅ | [event_object_movement.c:2139](src/event_object_movement.c#L2139) | `LoadObjectEventPalette`'s "not found" check compares the wrong value, so the condition is always true — failed palette lookups still load from index 0. Fixed: now looks up `sObjectEventSpritePalettes[i].tag` instead of comparing the index directly. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A10 | ✅ | [intro.c:886](src/intro.c#L886) | Title-screen sprite-palette array is missing its `{0}` terminator; `LoadSpritePalettes` reads past the array into the next `.rodata` section. Fixed: `{0}` terminator added unconditionally. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged, ROM data-only change). Not yet checked in-game (intro fight scene). |
| A11 | ✅ | [map_preview_screen.c:442](src/map_preview_screen.c#L442) | Map-name text-color array declared as `u8 color[0]` but 3 elements are written to it — out-of-bounds write on every map preview screen. Fixed: declared as `u8 color[3]`. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged, stack-only change). Not yet checked in-game. |
| A12 | ✅ | [mystery_gift_show_card.c:507](src/mystery_gift_show_card.c#L507) | Wonder Card stamp cleanup checks the wrong array index before freeing the second icon sprite. Fixed: now checks `stampSpriteIds[i][1]` (the index actually destroyed) instead of re-checking `[0]`. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A13 | ✅ | [oak_speech.c:1824](src/oak_speech.c#L1824) | Palette load during Oak's speech reads 48 colors past the intended range (currently masked by a later overwrite from player/rival sprite load). Fixed: `LoadPalette` size trimmed to the exact palette, no longer over-reading. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A14 | ✅ | [pokeball.c:1168](src/pokeball.c#L1168) | Poké Ball send-off: a sprite with a previously-paused affine animation doesn't get unpaused, so the "shrink into ball" animation doesn't replay correctly. Fixed: `affineAnimPaused = FALSE` now runs unconditionally before starting the anim. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A15 | ✅ | [pokedex_screen.c:3639](src/pokedex_screen.c#L3639) | Dex category page-turn background effect is called with the wrong page-position argument once the fade counter reaches 0. Fixed: now passes `0` (the final page position) instead of the unrelated case-state counter `data[0]`. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A16 | ✅ | [pokemon.c:2153](src/pokemon.c#L2153) | `currentHP` can unintentionally end up ≤ 0 after a subtraction instead of being clamped (e.g. when max HP decreases), incorrectly registering a living Pokémon as fainted. Fixed: clamp to 1 now runs unconditionally. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A17 | ✅ | [pokemon.c:5389](src/pokemon.c#L5389) | `ModifyStatByNature` return value stored in `u16` can overflow for any nature-boosted stat > 595/728 — doesn't happen with vanilla base stats but can with rebalanced ones. Fixed: intermediate `retVal` widened to `u32` (function's own return type stays `u16`, still plenty of range). Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A18 | ✅ | [roamer.c:206](src/roamer.c#L206) | `CreateRoamerMonInstance` passes a `u8` status field where `SetMonData` expects a `u32*`, reading 3 bytes past it (into cool/beauty/cute). Fixed: status copied into a proper `u32` local first. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A19 | ✅ | [text_window_graphics.c:55](src/text_window_graphics.c#L55) | `GetUserWindowGraphics` bounds-checks against a hardcoded `20` (RSE's frame count) instead of this game's actual smaller array size. Fixed: now checks `ARRAY_COUNT(gUserFrames)` (10 entries) instead of the hardcoded 20. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A20 | ✅ | [trade.c:2754](src/trade.c#L2754) | Without National Dex, trading an Egg isn't blocked by its own check; falls through to an unrelated (also broken) condition instead. Fixed: explicit `SPECIES_EGG` check now runs first (correct `CANT_TRADE_EGG_YET` reason code), dead `SPECIES_NONE` check removed. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game (needs a link-trade setup). |
| A21 | ✅ | [wild_pokemon_area.c:262](src/wild_pokemon_area.c#L262) | `IsSpeciesOnMap` checks the fishing encounter table using the land-encounter table's size — reads out of bounds. Fixed: now uses `FISH_WILD_COUNT`. Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game. |
| A22 | ✅ | [wireless_communication_status_screen.c:449](src/wireless_communication_status_screen.c#L449) | Union Room activity total omits any activity not in the three explicit groups (trade/battle/union) — undercounts the total. Fixed: now also adds `groupCounts[GROUPTYPE_TOTAL]`'s pre-existing value (its own copy-in from `groupCountBuffer`, evaluated before the assignment per C sequencing). Build verified (EWRAM 99.58%, IWRAM 91.02% — unchanged). Not yet checked in-game (Union Room feature). |

## Section B — documented in a comment, no fix written yet

These need an actual fix authored (no `#ifdef BUGFIX` branch exists for them).

| # | Status | Location | Bug |
|---|--------|----------|-----|
| B1 | ⬜ | [battle_transition.c:1306](src/battle_transition.c#L1306) | Two functions in a battle transition animation are incorrect (comment flags both, doesn't fully detail the fix). |
| B2 | ⬜ | [battle_transition.c:2495](src/battle_transition.c#L2495) | `WIN0V` register is set using the `win0H` field instead of `win0V`. |
| B3 | ⬜ | [easy_chat.c:463](src/easy_chat.c#L463) | A clear loop clears 64 bytes instead of the intended 64 bits (comment notes no observable gameplay effect). |
| B4 | ⬜ | [dodrio_berry_picking.c:608](src/dodrio_berry_picking.c#L608) | Minigame column-sharing table is asymmetric — column 7's difficulty is influenced by neighbors in a way column 6's isn't. |
| B5 | ⬜ | [evolution_scene.c:1195](src/evolution_scene.c#L1195) | The evolved Pokémon's cry incorrectly plays over the evolution sound effect. |
| B6 | ⬜ | [fieldmap.c:475](src/fieldmap.c#L475) | A loop writes past the bounds of the `mapView` array (size 0x100). |
| B7 | ⬜ | [field_effect.c:4027](src/field_effect.c#L4027) | Function return type should be `u32`, declared `void`. |
| B8 | ⬜ | [learn_move.c:741](src/learn_move.c#L741) | Wrong element of a `spriteIds` array is used (should be the second element). |
| B9 | ⬜ | [main.c:281](src/main.c#L281) | Key repeat doesn't work when pressing L in "L=A button" input mode. |
| B10 | ⬜ | [palette_util.c:392](src/palette_util.c#L392) | A comparison is never true for `maxBlendCoeff >= 8` (comment notes it's unobservable in vanilla gameplay). |
| B11 | ⬜ | [pc_screen_effect.c:49](src/pc_screen_effect.c#L49) | Possibly uses the wrong parameter (`speed` instead of an unused one). |
| B12 | ⬜ | [pokemon_jump.c:190](src/pokemon_jump.c#L190) | `tilemapBuffer[0x4000]` is far larger than needed. |
| B13 | ⬜ | [union_room.c:351](src/union_room.c#L351) | A `dst` output parameter is ignored; output always goes to `gStringVar4` instead. |
| B14 | ⬜ | [party_menu.c:3297](src/party_menu.c#L3297) | Memory leak (comment doesn't detail specifics yet — needs investigation). |

## Related, not tracked here yet

There's a **separate** `UBFIX` macro (also only forced on under `MODERN=1`) guarding ~10
undefined-behavior fixes across 8 files (`battle_ai_script_commands.c`, `dodrio_berry_picking.c`,
`event_object_movement.c`, `fieldmap.c`, `m4a.c` ×3, `pokemon_storage_system_misc.c`,
`union_room.c`) — mostly null-pointer dereferences that were harmless under the original
compiler but can crash under modern compilers like the GCC 16 toolchain this build uses. Worth
its own pass later given the crash risk, but out of scope for this file for now.
