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

## Levels

1. **Cell Block D** — footlocker on the pressure pad, then two breakers
2. **The Laundry** — blinking beams, a fetch quest, platforms over the chute
3. **The Yard** — kill three floodlights before Wire will cut the fence
4. **The Kitchen** — three weight pads, three crates, conveyors and steam
5. **Ventilation** — crouch ducts and three fan valves, one of which is a circuit trick
6. **The Infirmary** — a four-key interlock whose order is written on a crash cart
7. **The Workshop** — weight-interlocked gantries, solved in three moves
8. **The Sublevel** — a surging flood that is electrified until you cut the feeder
9. **The Roof** — precision platforming under sweeping searchlights
10. **The Wall** — power, card and counterweight, with three guards walking between them

## Technical notes

- Three.js r160 via import map (CDN), with an optional bloom pass that degrades
  gracefully if the addons fail to load
- Custom swept-AABB physics: move-and-slide, coyote time, jump buffering,
  moving-platform carry, pushable crates, crouch with headroom checks
- First-person by default: the camera sits inside the (hidden) head, the body
  turns with the view, and a small head bob and roll are driven by the walk cycle
- Characters are lofted through stacks of elliptical rings on a hand-made rig, so
  limbs taper and bulge the way real ones do; heads are a unit sphere pushed into
  a skull — tapered jaw, forward chin, brow ridge, flatter temples — and the hair
  reuses the same displacement so it sits on the skull rather than around it
- Animation is procedural: walk, run, jump, climb and crouch are all driven by
  sine phase rather than keyframes
- Puzzle logic is a channel system: devices set named channels, doors listen to
  boolean expressions over them (`['gatePower','gateCard','gateWeight']`)
- Guard vision is a cone test plus a slab raycast for line of sight; crouching
  shortens the range at which you are spotted
- Textures are drawn to canvases at load; box UVs are rescaled per-face so one
  shared material tiles correctly at any size
