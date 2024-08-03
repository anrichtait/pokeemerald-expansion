## Essential Files for Pokemon Emerald Expansion Decomp Projects

### Introduction

Editing Pokémon Emerald decomp projects can be quite confusing at times, especially with the expansion repo that has many additional features. This blog post aims to be a quick reference for myself and others, highlighting the most important files you need to edit. I will update this list as I learn and discover more.

I highly recommend using Porymap to find map, trainer, and sign names/scripts. It makes the process a lot easier in the beginning.

**Important Note:** These files are specific to the [Pokémon Emerald Expansion](https://github.com/rh-hideout/pokeemerald-expansion) version of the decomps.

### The Files

#### 1. File: `data/maps/map_name/scripts.inc` (or `.pory` if you set up Poryscript correctly)
This file is used to edit specific map scripts. Replace `map_name` with the map you want to edit, such as `RustboroCity` or `Rustboro_City_Gym`.

#### 2. File: `include/constants/opponents.h`
Define new trainers here. Keep in mind that if you add more than seven new trainers, you will need to start allocating and changing memory addresses. Unless you are a C wizard, I recommend sticking with the normal ones and repurposing them for your own needs.

#### 3. File: `src/data/trainer_parties.h`
Edit the party of a specific trainer by searching for their name. A good way to find specific trainer names is by using Porymap and clicking on them to see which script they use.

Below you can see the available flags to change:

```c
    .species = SPECIES_HAWLUCHA,
    .ball = ITEM_MASTER_BALL,
    .ability = ABILITY_MOLD_BREAKER,
    .friendship = 42,
    .gender = TRAINER_MON_MALE,
    .heldItem = ITEM_ORAN_BERRY,
    .isShiny = FALSE,
    .iv = TRAINER_PARTY_IVS(20, 20, 20, 20, 20, 20),
    //.ev = TRAINER_PARTY_EVS(00, 00, 00, 00, 00, 00)
    .lvl = 16,
    .moves = {MOVE_WING_ATTACK, MOVE_LOW_KICK, MOVE_DETECT, MOVE_HONE_CLAWS},
    .nature = NATURE_ADAMANT,
    .nickname = COMPOUND_STRING("berd"),
    //.dynamaxLevel = 5,
```

__NOTE:__ EV and IV spreads are as follows:
1. hp
2. atk
3. def
4. speed
5. sp.atk
6. sp.def

### 4. File: `src/data/trainers.h`
Edit the attributes of a specific trainer or gym leader here. Like with the `trainer_parties.h` file using porymap is a great way to find trainer names.

```c
[TRAINER_ROXANNE_1] =
{
    .trainerClass = TRAINER_CLASS_LEADER,
    .encounterMusic_gender = F_TRAINER_FEMALE | TRAINER_ENCOUNTER_MUSIC_FEMALE,
    .trainerPic = TRAINER_PIC_LEADER_ROXANNE,
    .trainerName = _("ROXANNE"),
    .items = {ITEM_POTION, ITEM_POTION, ITEM_NONE, ITEM_NONE},
    .doubleBattle = FALSE,
    .aiFlags = AI_FLAG_CHECK_BAD_MOVE | AI_FLAG_TRY_TO_FAINT | AI_FLAG_CHECK_VIABILITY | AI_FLAG_SMART_SWITCHING | AI_FLAG_WILL_SUICIDE,
    .party = TRAINER_PARTY(sParty_Roxanne1),
},
```

- **`.aiFlags`:** Lets you set the AI flags for the trainer, essentially making them smarter or more difficult. Some of these flags are really important, like the `AI_FLAG_WILL_SUICIDE` flag that allows trainers to use moves like "SELF-DESTRUCT". The `AI_FLAG_SMART_SWITCHING` flag is crucial as it makes trainers aware of type matchups, allowing them to switch if they face an unfavorable matchup.
- **`.items`:** Takes an array of items for the trainer to use, typically potions and similar items.
- **`.party`:** Specifies which party the trainer will use. This is important if you change party names in the `trainer_parties.h` file mentioned earlier.

#### 5. Directory: `include/config/`
This whole directory contains files with toggles added by the expansion team. For example, you can add the latest generation's Exp. Share functionality by editing these lines in the `include/config/items.h` file:

```c
// Exp. Share config
// To use this feature, replace the 0 with the flag ID you're assigning it to.
// Eg: Replace with FLAG_UNUSED_0x264 so you can use that flag to toggle the feature.
#define I_EXP_SHARE_FLAG        0               // If this flag is set, every Pokémon in the party will gain experience, regardless if they participated in the battle or not.
#define I_EXP_SHARE_ITEM        GEN_5           // In Gen6+, the Exp. Share was changed from a held item to a Key item that toggles the effect described above.
```

Modify the code as follows:

```c
#define I_EXP_SHARE_FLAG        FLAG_UNUSED_0x264
#define I_EXP_SHARE_ITEM        GEN_LATEST
```

Have a look through all the files in this directory. The comments explain most of the important details.

### 6. File: `src/starter_choose.c`

In this file, you can change the starter Pokémon. I haven't added more than three because I don't think it is necessary for my game, but I have changed the choices to:
- [Sprigatito](https://bulbapedia.bulbagarden.net/wiki/Sprigatito_(Pok%C3%A9mon))
- [Litten](https://bulbapedia.bulbagarden.net/wiki/Litten_(Pok%C3%A9mon))
- [Piplup](https://bulbapedia.bulbagarden.net/wiki/Piplup_(Pok%C3%A9mon))

### 7. File: `src/data/pokemon/species_info/gen_x_families.h`

This file is where you can change many aspects of each Pokémon. Below you can see the flags that I'm mostly interested in:

```c
[SPECIES_LITTEN] = {
    .baseHP        = 45,
    .baseAttack    = 65,
    .baseDefense   = 40,
    .baseSpeed     = 70,
    .baseSpAttack  = 60,
    .baseSpDefense = 40,
    .abilities = {ABILITY_BLAZE, ABILITY_NONE, ABILITY_INTIMIDATE},
},
```

I don't often change the base stats, but I have been tinkering with Litten's a little bit to make it more balanced as a starter. The above settings are all default, but I removed the Blaze ability so Intimidate is always the given ability. I think without it, Litten isn't a viable option compared to the others. I did something similar with Sprigatito, who now always has the Protean ability.

---

**This post is a work in progress, so stay tuned.**

