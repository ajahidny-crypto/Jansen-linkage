# Strandbeest Leg

A 3D-printed single leg from a Theo Jansen-style walking linkage, the same mechanism behind Strandbeest kinetic sculptures.

No metal, no electronics, no glue. Every joint is just a printed pin spinning in a printed hole. This is one leg only, hand-cranked at the pivot pin, built and tested before committing to the full 6-leg machine.

![Isometric view of the leg assembly and hardware](images/leg-assembly-iso.png)

![Top view of the leg assembly and hardware](images/leg-assembly-top.png)

<!-- TODO: add demo video/gif once I have the file named -->

## Why I made it

I didn't want to just trust the published Jansen numbers, or a picture of the linkage's shape, and print straight from that. I wanted to actually see the foot trace the right path with my own hands before committing to printing five more legs and a full frame around them.

Same lesson as a previous project where I guessed at some housing dimensions and it bit me: verify the thing physically before building everything else around it.

## How it works

Theo Jansen's linkage uses 12 fixed link lengths (a through m) connected in a specific layout. Turn the crank, and the far end of the chain (the foot) traces a closed loop with a flat section at the bottom, so it looks like it's walking instead of just swinging through the air.

This leg has 7 pivot joints: the crank pin (`X`), the fixed frame pivot (`Y`), and five more (`W`, `U`, `V`, `T`, `S`), where `S` is the foot.

## Design

| Part | Size |
|---|---|
| Standing height the scale targets (full 6-leg machine) | 190 mm |
| Frame pivot spacing (shaft hole to leg pivot hole) | 81.7 mm |
| Pin diameter | 5 mm |
| Pivot hole diameter | 5.3-5.4 mm |
| Link width | ~10% of the link's own length, 10 mm minimum |
| Link thickness | 6 mm |
| Boss diameter (around each pivot hole) | ~2.5x hole diameter |

| Link | Length (mm) | Connects |
|---|---|---|
| a | 80.0 | part of the frame shape (not a separate piece) |
| b | 87.4 | Y-W |
| c | 82.7 | Y-U |
| d | 84.4 | Y-V |
| e | 117.5 | W-V |
| f | 82.9 | V-T |
| g | 77.3 | U-T |
| h | 138.3 | T-S (longest link) |
| i | 103.2 | U-S |
| j | 105.3 | X-W |
| k | 130.3 | X-U |
| l | 16.4 | part of the frame shape (not a separate piece) |
| m | 31.6 | crank arm / throw radius, the one link you can resize on its own to change stride length |

PETG, not PLA. PLA cracks when you flex it to snap a joint together, PETG just bends.

![Front profile view of all the leg parts, sorted by length](images/leg-parts-profile.png)

## Getting it right

There's more than one way to mathematically solve this kind of linkage: different sign choices at each step give you different closed loops, and only one of them is the actual Jansen curve. I checked all of them and picked the one that matches it.

I also ran the numbers on a strict version of "foot touching the ground," treating it as a single point with zero give, and got worried for a bit: on paper, each leg only has a short window where it's perfectly flat, and those windows don't overlap cleanly even across a few crank stations. Turned out not to be a real problem. Real feet aren't mathematical points, and a rigid frame connecting six legs spreads the load around anyway. It's part of why I built and tested one leg by hand before committing to the full frame.

I also almost mounted the frame at the wrong angle. It's just a bar with two holes, a bigger one for the shaft and a smaller one for the leg pivot. At one point, going off a single coordinate snapshot from the simulation, I thought the line between those two holes had to sit at some specific tilt, and that having them level would be wrong. Turns out that's not true: I built it and tested it with both holes level, and it works fine. The shape the leg traces only depends on the link lengths, so rotating the whole frame and leg as one rigid piece doesn't change the shape, it just rotates it.

One more thing that mattered more than expected: deburring every hole and pin before assembly. Leftover layer lines from printing were the real reason joints got stiff, not the clearance numbers.

## Parts

| Qty | Part |
|---|---|
| 1 each | Links a through k (11 pieces) |
| 1 | Crank arm (link m) |
| 1 | Frame piece (bearing hole + pivot hole) |
| 7 | Pivot pins, 5 mm |
| 7 | Snap-caps (one per pin) |

## Putting it together

1. Print all the links (a through k), the frame piece, the crank arm, the pivot pins, and the snap-caps.
2. Deburr every hole and pin before assembling anything.
3. Pin the links together at each joint (`X`, `Y`, `W`, `U`, `V`, `T`, `S`) using the connections in the Design table.
4. Mount the frame piece onto the crank pin and the leg pivot.
5. Hand-crank it and check the foot traces a smooth path with a flat section at the bottom.

## Files

| File | Description |
|---|---|
| `README.md` | this file |
| `LICENSE` | project license |
| `images/leg-assembly-iso.png` | isometric view of the assembly |
| `images/leg-assembly-top.png` | top view of the assembly |
| `images/leg-parts-profile.png` | front profile view, parts sorted by length |

<!-- TODO: add the CAD and STL files -->
<!-- TODO: add the demo video/gif -->

## Tools and materials

- Fusion 360 (CAD)
- Bambu Lab P1S (3D printer)
- PETG filament

## Future plans

- Print the other 5 legs and the 3 crank disks.
- Build the full frame (2 shaft bearings + 6 leg pivots) and test the whole thing standing and walking.
- Maybe add a small gear pair at the hand crank for easier cranking, haven't decided if that's just for testing or the final build.

## License

This project is open source. See LICENSE for the terms.
