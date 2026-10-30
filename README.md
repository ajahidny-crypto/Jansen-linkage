# Single-Leg Jansen Linkage Prototype

A 3D-printed single leg off a Theo Jansen-style walking linkage, basically the mechanism behind those "Strandbeest" kinetic sculptures. No metal, no electronics, no glue: every joint is just a printed pin in a printed hole. This repo is just the one leg for now, hand-cranked at the pin, built and tested before committing to the full 6-leg machine.

## Demo

![leg walking](images/leg-walking.gif)

Full clip: [videos/leg-walking.mp4](videos/leg-walking.mp4)

## Renders

Top view, the linkage plus the frame, crank arm, and loose pins/washers off to the side:

![top view of the leg assembly and hardware](images/leg-assembly-top.png)

Same thing from an isometric angle, easier to see how the links actually stack on top of each other at the joints:

![isometric view of the leg assembly and hardware](images/leg-assembly-iso.png)

Front-on profile view, good for checking link thickness and seeing all the parts sorted by length:

![front profile view of all the leg parts](images/leg-parts-profile.png)

## What this is

Theo Jansen's linkage runs on 12 fixed link lengths (labeled a through m) hooked up in a specific layout that traces a closed foot path with a flat part at the bottom, the part where the foot's walking flat along the ground instead of swinging through the air. Get the lengths right and that shape just happens from the geometry. Get them wrong, or hook the links up wrong, and you get some closed loop that looks nothing like walking.

This leg has 7 pivot joints: the crank pin (`X`, where the hand crank drives it), the fixed frame pivot (`Y`), and five more (`W`, `U`, `V`, `T`, `S`, where `S` is the foot).

## Link lengths (verified, scaled to my build)

I did some research to check Jansen's numbers were actually right, and they checked out clean everywhere I looked. I scaled everything by 2.1051mm per raw unit to get a 190mm standing height, which puts it at a normal desktop size.

| Link | Raw units | Length (mm) | Connects |
|---|---|---|---|
| a | 38.0 | 80.0 | part of the frame shape (not a separate piece) |
| b | 41.5 | 87.4 | Y-W |
| c | 39.3 | 82.7 | Y-U |
| d | 40.1 | 84.4 | Y-V |
| e | 55.8 | 117.5 | W-V |
| f | 39.4 | 82.9 | V-T |
| g | 36.7 | 77.3 | U-T |
| h | 65.7 | 138.3 | T-S (longest single link) |
| i | 49.0 | 103.2 | U-S |
| j | 50.0 | 105.3 | X-W |
| k | 61.9 | 130.3 | X-U |
| l | 7.8 | 16.4 | part of the frame shape (not a separate piece) |
| m | 15.0 | 31.6 | crank arm / throw radius, the one link you can resize on its own to change stride length without messing up the gait shape |

Frame pivot spacing (shaft hole to leg pivot hole, straight line): 81.7mm.

A couple other numbers that came out of the simulation instead of being something I picked: stride length is 174.6mm per crank turn, and the full swing of all the joints (not just the foot) covers roughly 278mm front-to-back and 222mm up-down. That's bigger than the standing height because some of the middle links swing higher and further out than the foot does.

## How I checked it before printing anything

Instead of trusting a picture or my memory of the shape, I wrote a simulation to actually trace the foot path through a full crank turn and check that it closes up clean and does the flat-bottom thing it's supposed to do.

Closure error: 0.000000mm. No gaps, no jumps.

Before printing anything, I also laid out all the joint positions at one crank angle and measured the distance between every pair that's supposed to be a link. If any of them didn't match the length in the table above, I'd have caught the mistake right there instead of after printing a bunch of plastic. Everything matched.

## Joints: free-spinning, not press-fit

Every joint here has to spin freely:

- Pins are 5mm, holes are 5.3-5.4mm, so there's a bit of play.
- One end of the pin has a slightly domed head printed right onto it so it can't pull out sideways, and the other end gets a small snap-cap after assembly.
- Deburring every hole and pin before putting it together mattered way more than hitting the exact clearance number. Leftover layer lines from printing were the real reason joints got stiff, not the sizing.

## Material and cross-section

PETG, not PLA, for every link. PLA's too brittle for snap-fit pins, it cracks when you flex it to snap a joint together. PETG bends instead of snapping, which is exactly what this needs.

Cross-section: width is about 10% of the link's length (minimum 10mm so even the short ones stay sturdy), thickness is 6mm, with a thicker boss around each hole (about 2.5x the hole size) so the hole doesn't just tear out. I did some research into other printed builds and saw people having to double their link thickness after parts bent under load, so I just started thick instead of finding that out the hard way.

## Issues I ran into

Not every closed loop you get out of the math is the right one. There's more than one way to solve this kind of linkage, different sign choices at each step give you different closed loops, and only one of them is the real Jansen curve. I checked all of them and picked the one that actually matches it, not just whichever one closed first.

The math looked worse than it actually is. If you treat "touching the ground" as a strict point check with zero give, each leg only has a short window where it's perfectly flat, and on paper those windows don't all overlap even with a few crank stations. That had me worried for a second, but it's not actually a problem: real feet aren't mathematical points, they flex a little and have some actual contact area, and the frame connecting all the legs spreads the weight around anyway. It's basically why I built and tested one leg by hand before committing to the full 6-leg frame instead of just trusting the math.

I almost overcomplicated the frame angle for no reason. The frame's just a bar with two holes, a bigger one for the shaft and a smaller one for the leg pivot. At one point, going off a single snapshot from the simulation, I thought the line between those two holes had to sit at some specific tilt (about 11.6°), and that having them level would be wrong. Turns out that's not true. I built it and tested it with both holes level and it works fine. The shape the leg traces only depends on the link lengths, so rotating the whole frame and leg as one rigid piece doesn't change the shape at all, it just spins it. That "tilt" was just an artifact of how the simulation's coordinates were set up, not an actual requirement, and the real build proved it.

## What's built and verified so far

- One leg, all 7 joints, hand-cranked with a temporary lever. Spins freely through a full turn.
- The frame piece (bigger hole + smaller hole, level) prints fine and holds the leg's pivot right where it needs to be.

## Open questions / next steps

- Thinking about adding a small gear driving a bigger gear at the hand crank so it's easier to turn (slower, but less effort). Still deciding if that's just for this one test leg or if it should go on the real machine too. If it's for the real machine, it needs to go once at the shared shaft's input, not on every leg's own crank pin, or it'll mess up the 120° timing between the three stations that currently comes for free since they're all on one shaft.
- Print the other 5 legs and the 3 crank disks, build the full frame, and make sure the whole thing stands and walks without binding before adding a permanent crank handle.

## Where these numbers come from

I don't have the actual scripts or output files from when I built this, lost them along the way. The numbers and findings above are what I documented while actually building it, not something I re-ran just now.
