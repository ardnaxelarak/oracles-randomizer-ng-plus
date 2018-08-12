# Oracle of Seasons randomizer

This program reads a Zelda: Oracle of Seasons ROM (JP version only) shuffles
the locations of key items and seeds, randomizes the default season for each
area, and writes the modified ROM to a new file. It also bypasses essence
checks for overworld events that are necessary for progress, so the dungeons
can be done in any order that the randomized items facilitate. However, you do
have to collect all 8 essences to get the Maku Seed and finish the game.

The randomizer is relatively new and under active development, so consider it
"beta" for now. See the [issue
tracker](https://github.com/jangler/oos-randomizer/issues) for known problems.


## Usage

The randomizer uses a command-line interface, and I currently have no plans to
implement a graphical one. It's a simple program (from the user's perspective),
and command lines are not very hard.

The normal usage is `./oos-randomizer oos_original.gbc oos_randomized.gbc` (or
whatever filenames you want), but there are additional flags you can pass
before the filename arguments to fine-tune the randomization, as displayed in
the usage (`./oos-randomizer -h`) message:

    Usage of ./oos-randomizer:
      -forbid string
            comma-separated list of nodes that must not be reachable
      -goal string
            comma-separated list of nodes that must be reachable (default "done")
      -maxlen int
            if >= 0, maximum number of slotted items in the route (default -1)
      -seed string
            specific random seed to use (32-bit hex number)
      -update
            update randomized ROM to this version
      -verbose
            print more detailed output to terminal

Note that some combinations of these flags can result in impossible conditions,
like `-goal 'd1 essence' -forbid 'ember seeds'`. If you specify an early goal,
like `enter d3`, you'll probably want to use `-maxlen` or `-forbid` to make
sure the randomizer doesn't make the floodgate key, for instance, the D8
dungeon item. See further below for an abbreviated list of possible `-goal` and
`-forbid` nodes.

Regardless of the value of `-maxlen`, the randomizer will place items in all
available slots. The flag just limits the number of slotted items that are
*necessary* in order to reach the goal(s).


## Download

You can download executables for Windows, MacOS, and Linux from the
[releases](https://github.com/jangler/oos-randomizer/releases) page.


## Randomization notes

Most inventory items (equippable and non-equippable) that are necessary to
complete a casual playthrough are shuffled, with some exceptions:

- Purchasable items (bombs, shield, and strange flute) are not shuffled.
- The ribbon and pirate's bell are not shuffled (but the rusty bell is).

**Items are only placed in locations where you would normally obtain another
key item.** Speedrunners should note that the Subrosian dancing prize could be
important.

Seed trees are also shuffled, and the satchel and slingshot will start with the
type of seeds on the tree in Horon Village.


## Other notable changes

Other small changes have been made for convenience, to simplify randomization
logic, or to prevent softlocks. The most notable are:

- **Save and quit is replaced with a warp to the seed tree in Horon Village,**
  except if you do it from a game over.
- Mystical seeds grow in all seasons.
- Seeds can be collected if the player has either slingshot or satchel.
- The cliff between Eastern Suburbs and Sunken City has stairs instead of a
  spring flower.
- Rosa doesn't appear in the overworld, and her portal is activated by default.
- The diving spot at the south end of Sunken City is removed.


## Potentially useful goal/forbid nodes

- `done`, meaning defeating Onox
- `dX essence`, where X is a number from 1-8
- `boomerang L-1`
- `sword`
- really just look at the files in the "prenode" folder if you want more

Remember that you can specify multiple nodes as goals/forbids by separating
them with commas. Also remember to quote the strings lol
