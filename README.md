# Single-Leg Jansen Linkage Prototype

A fully mechanical, 3D-printed single leg of a Theo Jansen-style walking linkage, the mechanism behind "Strandbeest" kinetic sculptures. No metal, no electronics, no adhesive: every joint is a printed pivot pin in a printed hole. This repo currently covers one leg, hand-actuated at the crank pin, built and tested as a proof of concept before committing to the full multi-leg walking machine.

## Demo

![leg walking](images/leg-walking.gif)

Full clip: [videos/leg-walking.mp4](videos/leg-walking.mp4)

## What this is

Theo Jansen's linkage uses 12 fixed link-length ratios (conventionally labeled a through m) connected in a specific topology to trace a closed, non-self-intersecting foot path with a distinctive flat-bottomed "stance" phase: the part of the gait where the foot walks flat along the ground instead of swinging through the air. Get the ratios right and the shape is guaranteed by the geometry; get them wrong, or connect the links incorrectly, and you get a closed loop that looks nothing like a walking gait.

This leg has 7 pivot joints: the crank pin (`X`, where the hand crank drives the leg), the fixed frame pivot (`Y`), and five more internal joints (`W`, `U`, `V`, `T`, `S`, where `S` is the foot).

## Link lengths (verified, scaled to my build)

I cross-checked Jansen's "holy numbers" against 5+ independent sources (Wikipedia, Wikibooks, an OpenSCAD reference implementation, an academic paper, and a 2026 arXiv preprint): zero discrepancy between them. I then scaled everything by 2.1051 mm per raw unit to hit a 190mm standing height, a tabletop build inside the common 150-200mm range used by published printable Strandbeest models.

| Link | Raw units | Length (mm) | Connects |
|---|---|---|---|
| a | 38.0 | 80.0 | fixed frame geometry (Z-Y relationship, not a separate bar) |
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
| l | 7.8 | 16.4 | fixed frame geometry (Z-Y relationship, not a separate bar) |
| m | 15.0 | 31.6 | crank arm / throw radius, the one link you can scale independently to change stride length without changing the gait shape |

Frame fixed-pivot spacing (shaft-bearing point to leg-pivot point, straight-line): 81.7mm.

Other numbers that came out of simulation rather than being design choices: stride length 174.6mm per full crank rotation, and a full swept envelope (all joints, across one full rotation) of roughly 278mm fore-aft x 222mm vertical. That's bigger than the standing height because some intermediate links swing higher and further than the foot itself.

## How I checked it before printing anything

Rather than trusting a picture or memory of the linkage's shape, I wrote a numerical simulation (circle-intersection kinematics, swept through a full 360° crank rotation) and validated it against a known property of the real mechanism: a smooth, closed, non-self-intersecting foot path with a flat-bottomed stance phase spanning roughly 38% of the stride width.

Closure error on the simulated path: 0.000000mm. No discontinuities.

Before printing, I also laid out a coordinate validation sketch: exact joint positions at one reference crank angle, with the known link length between every connected pair of joints. If a link measured between two points in the sketch hadn't matched the length in the table above, I'd have caught it immediately, before any plastic was printed. Every link matched.

## Joints: free-spinning, not press-fit

Unlike press-fit designs, every joint here needs to spin freely:

- Pin diameter: 5mm. Hole diameter: 5.3-5.4mm (0.3-0.4mm total diametral clearance).
- Retention: a slightly domed pin head on one end (printed as part of the pin), with the other end secured by a small snap-cap after assembly.
- Deburring every hole and pin before assembly mattered more than any dimensional tweak. Printed layer-line burrs were the main source of joint stiffness, not clearance error.

## Material and cross-section

PETG, not PLA, for every link. PLA is too brittle for the snap-fit pin retention here and splits under repeated flexing as a joint is snapped together. PETG's extra flex is exactly what this joint style needs.

Cross-section: link width roughly 10% of the link's own length (minimum 10mm, so even the short l-scale links stay sturdy), thickness 6mm, with a circular boss (~2.5x hole diameter) around each pivot hole for strength where load concentrates. This sizing came from a documented failure mode in other printable builds: one project had to double their link thickness after the originals visibly bent under load, so I started generous rather than minimal.

## Issues I ran into

Not every mathematically closed loop is the right one. The circle-intersection method for this linkage has multiple valid branch solutions: different sign choices at each step produce different, but still mathematically closed, loops. I checked all of them and picked the one that reproduces the known flat-bottomed Jansen curve, not just any closed shape.

Idealized point-contact math looked worse than reality. Treating "ground contact" as a strict, zero-compliance point check showed individual legs with short, non-overlapping flat-contact windows. On paper, nothing guarantees a leg is in that strict flat zone at every instant, even with several crank stations. That's a real result, not a mistake, but it's not evidence the design fails: real feet have a finite contact patch and slight compliance, and a rigid frame shares load across legs rather than requiring any single leg to be in perfect isolated contact. It's the same reason legged robots lean on momentum and mild compliance instead of perfect static balance at every instant. Practically, it's why I built and tested one leg by hand before committing to the full multi-leg frame, instead of trusting the math alone.

I almost over-constrained the frame mount angle. The frame is a simple bar with two holes: a larger bearing hole for the crank shaft and a smaller pivot hole for the leg. Early on, working backward from a single coordinate snapshot of the simulation, it looked like the line between those two holes had to sit at a specific ~11.6° tilt, and that the two holes being level with each other would be wrong. The physical, tested build says otherwise: the frame works fine with both holes level. The shape of the leg's traced path is fixed entirely by the link lengths, and rotating the whole frame-and-leg assembly as a rigid body doesn't change that shape, it just rotates it. The simulation snapshot's "tilt" was an artifact of that one coordinate system, not a real mounting requirement, and the verified build was right to override it.

## What's built and verified so far

- One leg, all 7 joints, hand-actuated at the crank pin with a temporary lever. The shaft spins freely through the full rotation.
- The frame piece (bearing hole + pivot hole, holes level) prints and fits, and holds the leg's fixed pivot correctly.

## Open questions / next steps

- Whether to add a small-driver/large-driven gear pair at the hand crank for torque multiplication (easier cranking, slower turn per rotation), and if so, whether that belongs on this single test leg only or at the shared crankshaft's single input point once the full machine is built. Putting a separate gear train on every leg's own crank pin in the full build would break the free 120-degree phase synchronization that currently comes from all crank disks sharing one D-shaft, so any gearing meant for the final multi-leg machine should sit once, at the shared shaft's input, not per leg.
- Build the remaining 5 legs and the crank disks, assemble the full 3-station frame, and verify the whole machine stands and walks without binding before adding the permanent crank handle.

## Where these numbers come from

The simulation scripts and raw output files from development aren't included in this repo snapshot. The numbers and findings above come from what I documented while building, not from rerunning the original code.
