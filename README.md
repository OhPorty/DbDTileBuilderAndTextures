# OhPorty's DbD Blender Add-ons

Two Blender 5 extensions for Dead by Daylight exports from [FModel](https://fmodel.app):

- **OhPorty's DbD Blueprint/Tile Builder** rebuilds tiles, blueprints, full procedural maps and uMaps.
- **OhPorty's DbD Material Maker** builds the game's materials (layers, tiling, normals, alpha) on imported meshes.

You'll find both in the 3D Viewport sidebar (press **N**), under **Porty's BPs** and **Porty's Mats**.

---

## Installing

You need Blender 5.0 or newer.

1. **Remove any old copy first.** If you installed these add-ons from a zip before, remove them in **Edit → Preferences → Add-ons**. The old copy and the extension can't both be enabled.
2. **Allow online access.** In **Preferences → System**, turn on **Allow Online Access**. Blender needs it to download the add-ons and check for updates.
3. **Add this repository.** In **Preferences → Get Extensions**, open **Repositories** (the drop-down at the top right), click **+**, choose **Add Remote Repository** and paste:

   ```
   https://raw.githubusercontent.com/<you>/<repo>/main/index.json
   ```

4. **Install.** Search Get Extensions for **DbD** and click **Install** on each add-on you want.

### Updating

Blender checks this repository for new versions. When one is out, an **Update** button appears in **Preferences → Get Extensions**. You can also press **Check for Updates** there yourself. Your settings in saved .blend files stay as they are.

---

## Exporting from FModel

**Read this before your first build.** Almost every "missing object" report comes from an incomplete export. The add-ons can only build what you've exported, and the game spreads what a single tile needs across several folders.

### 1. FModel settings

In FModel's **Settings**:

- Turn on **Serialize Script Bytecode** and **Keep Directory Structure**.
- Set **Mesh Format** to **ActorX (psk)** or **UEFormat (uemodel)**.
- Note the **Output Directory**. That's the folder you pick as **Exports Folder** in the add-ons.

### 2. Export these folders in full

Right-click each folder in FModel and choose **Export Folder**. Some folders need **both** export types.

| Folder | Properties (.json) | Models | What it's for |
|---|:---:|:---:|---|
| `Content/Blueprints` | ✅ | | Every tile, prop setup and gameplay object (pallets, lockers, generators, hooks, windows, totems, chests, hatch) |
| `Content/Meshes` | ✅ | ✅ | **Both.** Models gives the meshes, materials and textures. Properties gives the blueprints stored in the mesh folders |
| `Content/Data` | ✅ | | The game's data tables: hook and pallet counts, tile rules and realm settings |
| `Content/ProceduralLevelGeneration` | ✅ | | The full-map layouts |
| `Plugins/Realms` | ✅ | ✅ | **Both.** The newer realms (see below) |

#### The tile blueprints alone aren't enough

A tile's blueprint is only the outline. The tile places smaller blueprints, like dumpsters, shelves, signs and picnic tables, and many of those **live in the mesh folders**, not under `Content/Blueprints`. There are over 700 of them in `Content/Meshes` alone. If you export `Content/Meshes` only as Models, you get the meshes but not those blueprints, and parts of every tile go missing.

Don't rely on FModel's search for `BP_` either. Several core gameplay blueprints aren't named `BP_`, such as `GeneratorStandard`, `ClosetStandard` (lockers) and `WindowStandard` (vaults), so a search misses them. Export the whole folders.

#### The newer realms are in Plugins

The newer realms, starting with **Liminal**, aren't under `Content` at all. Each one is a separate plugin with its own Blueprints, Meshes, Data and map layouts:

```
Plugins/Realms/Realm_26Liminal/Content/...
```

If you export only `Content`, those realms build with missing pieces or not at all. Export the whole **`Plugins/Realms`** folder **both** ways, as Properties (.json) and as Models, so every realm's folders come with it, including realms added in future updates.

### 3. After a game update

Export the same folders again. Exporting over your old files is fine.

---

## Building

In **Porty's BPs**, set **Exports Folder** to FModel's Output Directory, then:

- **Full map:** pick a map in `ProceduralLevelGeneration/TiledMaps/Completed` (for example `16-Wales/Wal_Level_01.json`), set a seed, and press **Build Full Map**. The same seed always builds the same map.
- **Single tile or blueprint:** pick any `BP_` file, such as a tile in `Content/Blueprints/Tiles`, and press **Build Blueprint**. A single tile shows every spawn point. Spawn counts only apply to a full map.
- **uMap:** pick a level in `Content/Maps` and press **Import uMap**.

Then select the objects and press **Build Materials** in **Porty's Mats**.

Each build gets its own collection, with groups such as Structures, Props, Trees and Plants, Gameplay and Lights. Untick a group in the Outliner to hide it.

---

## Something's missing?

1. Open **Window → Toggle System Console** and build again. The console lists every file the build couldn't find.
2. Export what it lists, or re-export the folders above in full, then build again.
3. For materials, **Porty's Mats → Tools → Show Report** lists missing textures. Export the meshes that use them again, press **Rescan Export Folder**, then **Build Materials**.

If the console is clean but something's still missing:

- Check the object's checkbox in the **Gameplay Objects** panel, such as Pallets or Lockers. These checkboxes are saved in each .blend file, so an old file can have one switched off.
- Make sure the add-on is up to date in **Preferences → Get Extensions**.

If it's still missing, report it with the map and seed, or the tile name, plus the console output.

---

## Credits

`dbd_procgen.py` in the Tile Builder is a Python port, with changes, of the Dead by Daylight procedural map code in [CUE4Parse](https://github.com/FabianFG/CUE4Parse), used under the Apache License 2.0. See `THIRD_PARTY_NOTICES.txt` in the add-on.

Both add-ons are licensed under GPL-3.0-or-later.
