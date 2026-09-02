# IRON GATES

A 3D prison-escape puzzle platformer that runs in the browser. Single HTML file,
no build step, no assets — every texture, character and sound is generated at
load time.

**Play:** https://calebh314.github.io/iron-gates/

## The idea

You are an inmate working your way out of a twelve-floor prison. Each of the ten
levels is a self-contained puzzle room that ends at a large door, and that door
will not open until you have solved the mechanism feeding it — a weight-held
pad, a cut breaker, a keycard, a four-step interlock.

You cannot do it alone. Every level has prisoners who will talk to you, and
several of them are holding the thing you need or the piece of knowledge that
makes the room make sense.

## Controls

| | |
|---|---|
| `W` `A` `S` `D` | Move |
| Mouse | Look (click the page to capture the cursor) |
| Settings | First person by default; third person is available if you prefer it |
| `Space` | Jump |
| `Shift` | Sprint |
| `Ctrl` / `C` | Crouch — the only way through vents |
| `E` | Talk, pull, press, swipe, pick up |
| `W` / `S` on a ladder | Climb |
| `R` | Respawn at the last checkpoint |
| `Esc` | Pause |

## Structure

101 levels: ten hand-built, ninety generated across nine themed wings, then a
finale. Generated levels vary by **shape** (`doors` sequential, `open` roam the
hall in any order, `reverse` start at the far end and work back), by chamber
count, and — past the halfway mark — by carrying two problems per chamber
instead of one. Difficulty is a global ramp across all ninety rather than
resetting each wing, so guards, doorway beams and searchlights appear as you get
deeper. All 101 verified structurally distinct.

**Level 101 — The Long Night** is the finale: five zones, eleven separate locks
on one gate (a weight pad, three cage releases, two keycards, a four-step
interlock, three breakers and a counterweight), two guards and two searchlights.

## Unlock code

Typing **951413** on any menu screen, or entering it in Settings, opens all
100 levels at once.

## Difficulty

Settings has three presets. They scale the things that actually decide whether a
jump or a guard is fair, and can be changed mid-level:

| | jump apex | edge forgiveness | guards / hazards |
|---|---|---|---|
| Relaxed | 2.22 m | 312 ms coyote | see less, lit for less, platforms wait ~2x |
| Standard | 1.52 m | 120 ms | as designed |
| Hard Time | 1.38 m | 74 ms | sharper eyes, hazards lit longer |

On Relaxed a running jump clears about 6.5 m of flat ground, which is enough to
cross the roof gaps without waiting for the moving platforms at all.

## Wings

100 levels in ten themed wings of ten. Wing 1 is hand-built — the original
escape, from your cell to the wall. Wings 2–10 are generated from a fixed seed,
so a given level is always the same level.

| | wing | theme |
|---|---|---|
| 1 | The Old Wing | the original break: cells, laundry, yard, kitchen, vents, infirmary, workshop, sublevel, roof, wall |
| 2 | The Foundry | heat, slag and moving steel |
| 3 | The Archive | paper, dust and motion sensors |
| 4 | Hydroponics | wet floors and grow lamps |
| 5 | The Kennels | they walk the dogs at four |
| 6 | Cold Storage | everything in here keeps |
| 7 | The Quarry | open sky, no cover |
| 8 | Signal Tower | straight up, all the way |
| 9 | The Deep | below the water table |
| 10 | Freedom Road | the last mile |

A generated level is a chain of 3–7 chambers laid end to end. Each chamber holds
one self-contained challenge — weight pads, breakers, a gap with platforms,
ferries, blinking beams, a keycard, a patrolling officer, a four-button
interlock, a crouch duct, or a prisoner who will help — and the channel it
produces opens the door to the next. Because each chamber is solvable alone and
the chain is linear, the level always completes. That invariant is checked: all
100 levels build with exactly one exit, no stuck spawns, and every door channel
produced by something actually present in the level.


## Technical notes

- Three.js r160 via import map (CDN), with an optional bloom pass that degrades
  gracefully if the addons fail to load
- Custom swept-AABB physics: move-and-slide, coyote time, jump buffering,
  moving-platform carry, pushable crates, crouch with headroom checks
- First-person by default: the camera sits inside the (hidden) head, the body
  turns with the view, and a small head bob and roll are driven by the walk cycle
- Characters are built entirely from axis-aligned boxes on a hand-made rig —
  35 blocks for a boy, 41 for a girl, head about a quarter of body height
- Animation is procedural: walk, run, jump, climb and crouch are all driven by
  sine phase rather than keyframes
- Puzzle logic is a channel system: devices set named channels, doors listen to
  boolean expressions over them (`['gatePower','gateCard','gateWeight']`)
- Guard vision is a cone test plus a slab raycast for line of sight; crouching
  shortens the range at which you are spotted
- Textures are drawn to canvases at load; box UVs are rescaled per-face so one
  shared material tiles correctly at any size
