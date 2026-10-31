# Legacy of Rust + Doom Deluxe Beta1 Compatibility Patch

Adds a small compatibility pk3 that lets Legacy of Rust (id1.pk3) run with
Doom Deluxe Beta1 without the mod stealing ID1's pickups.

<img width="1920" height="1080" alt="Screenshot_Doom_20261031_150256" src="https://github.com/user-attachments/assets/661edcfd-70c1-440b-affd-a5550796252f" />
<img width="1920" height="1080" alt="Screenshot_Doom_20261031_152045" src="https://github.com/user-attachments/assets/6315bc7b-477f-4e1f-8168-c95b917698b6" />


## The problem

ID1 places its new weapons and fuel on the old Doom editor numbers:

- 2004: Incinerator, reusing the plasma rifle number
- 2006: Calamity Blade, reusing the BFG9000 number
- 2047: Fuel Can, reusing the cell charge number
- 17: Fuel Tank, reusing the cell pack number

Doom Deluxe remaps these same numbers in its ZMAPINFO DoomEdNums block.
MAPINFO is applied after embedded DEHACKED, so in the final game the pickups
spawn as Doom Deluxe's plasma rifle, BFG9000 and cell ammo instead of ID1's
weapons and fuel.

On top of that, ID1's DEHACKED sets the shared cell ammo to max 150 and
rescales the ammo given by every weapon that uses cell, including Doom
Deluxe's plasma rifle and BFG9000.

## What the patch changes

ZMAPINFO.txt, editor number overrides:

- 2004 -> VR_Incinerator
- 2006 -> VR_CalamityBlade
- 2047 -> FuelAmmo
- 17 -> Gascan

DEHACKED, restores the shared ammo and pickup text:

- Cell ammo: max 300, backpack max 600, pickup amount 20
- Pickup strings: GOTBFG9000, GOTCELL, GOTCELLBOX, GOTPLASMA

## Result

- The Incinerator pickup gives Doom Deluxe's flamethrower.
- The Calamity Blade pickup gives Doom Deluxe's Calamity Blade.
- Fuel cans and fuel tanks feed Doom Deluxe's FuelAmmo, max 150.
- Doom Deluxe's plasma rifle and BFG9000 keep their normal cell capacity.
- ID1 enemies and decorations are not touched.

Both ID1 weapons end up in weapon slot 8, the slot Doom Deluxe uses for
them, not in slots 5 and 6 as in the original ID1 release.

## Load order

The patch must load after both mods:

1. doom2.wad
2. id1.pk3
3. Doom_Deluxe_Beta1.pk3
4. ID1_DoomDeluxe_Compat.pk3

## Limitation

The patch changes the global editor numbers 2004, 2006, 2047 and 17, the
same numbers every Doom map uses for plasma, BFG and cell ammo. Keep it in
a loadout used only for id1.pk3 with Doom Deluxe. With other mapsets the
plasma rifle, BFG9000 and cell pickups will be remapped too.

## Package contents

- ID1_DoomDeluxe_Compat.pk3: ready to load
- src/ZMAPINFO.txt: editor number overrides
- src/DEHACKED: shared ammo and strings
- build.py: rebuilds the pk3 from src
- README.txt: purpose, load order, checks
