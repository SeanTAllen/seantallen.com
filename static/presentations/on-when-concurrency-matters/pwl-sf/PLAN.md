# Behaviour-Oriented Concurrency talk — planning

Working draft. Captures decisions made so far so we can pick up
across sessions without losing context.

## Source material

- Paper: *When Concurrency Matters: Behaviour-Oriented Concurrency*
  (Cheeseman, Parkinson, Clebsch, Kogias, Drossopoulou, Chisnall,
  Wrigstad, Liétar — OOPSLA 2023)
- PDF: https://marioskogias.github.io/docs/boc.pdf
- Real-world Python implementation: https://microsoft.github.io/bocpy/
- Sean is in the paper's acknowledgements (§8) for "interesting
  questions and feedback"

## Content outline (broad)

The arc: explain the actor model thoroughly so its strengths land,
surface its hidden pain point (multi-actor atomic operations), then
introduce BOC as the model that keeps the strengths AND solves the
pain.

### Opening — Origins

- Matt Parkinson + Sylvan Clebsch conversations: "what would Pony
  need to be a good database language?"
- That question led to BOC
- BOC is being implemented in Verona (Microsoft)

### Section 1 — The actor model

Property-focused, not Pony-focused. Pony is named once at the
genesis above; from here on the section is about the actor model
generically. Audience must internalize "this is genuinely good"
before the gap is surfaced.

- Actor model basics: actors message each other, process messages
  sequentially, protect resources
- Safety: data race freedom
- What a data race actually is (the four-part definition)
- How the actor model prevents data races (no globals, sequential
  processing, actors-as-sync-ops)
- The Achilles' heel: coordinating across multiple actors atomically
  requires bespoke protocols. This is the actor model's hidden pain.

### Section 2 — BOC

- Cowns: concurrent owners, protect separated data, available /
  acquired states
- Behaviours: units of concurrent execution, spawned with cowns +
  closure
- The `when` keyword — what it actually looks like
- Happens-before: ordering relation built from spawn-before +
  overlapping cowns
- Execution rules: behaviour runs when all cowns available + all
  happens-before predecessors done; cowns acquired atomically;
  exclusive access; can't acquire more or release mid-flight
- Two big guarantees: data-race free AND deadlock free, both by
  construction

### Section 3 — The example: a multi-table database

One example, built up across many slides. Users + orders +
stock. Operation: "place an order" touching all three
atomically.

The contrast goes three ways:

1. **C++/Rust**: fast, but hand-rolling locks, ordering matters,
   deadlock risk
2. **Actor model**: safe, scalable, no data races — but multi-actor
   atomic ops are a *protocol*, not a primitive
3. **BOC**: safe, scalable, AND multi-resource ops collapse to
   `when (users, orders, stock) { ... }`

Loops back to the genesis: Matt and Sylvan asked what Pony would
need to be a good DB language. BOC is the answer.

### Closer — bocpy

- Real-world Python implementation: https://microsoft.github.io/bocpy/
- Sean's note: "it's not what you might expect" — flesh out in
  delivery

## Cut from scope

Paper sections we are NOT covering:

- §2.5 / §2.7: Order Matters / Cost of Order (Dining Philosophers
  degenerate case)
- §3: Formal semantics (STEP / SPAWN / RUN / END inference rules)
- §4: Implementation details (DAG, MCS-queue, C++ runtime)
- §5: Savina benchmark numbers in detail
- §6: Related work survey
- The paper's bank-account running example (replaced by the
  multi-table DB example)

## Visual identity

### Direction

1960s "World of Tomorrow" optimistic atomic-age content rendered
in the visual vocabulary of mid-century children's-book
illustration. Think the cover of a 1962 storybook about the
future.

### Locked decisions

- **Light vs dark**: light, optimistic. Not Cold War anxious.
- **World vs character**: world-heavy. Environments, architecture,
  objects — not characters mugging.
- **Color**: flat saturated, not painterly. Mid-century printed
  palette (turquoise / coral / mustard / cream as a starting
  point).
- **Outlines**: visible black ink outlines defining shapes.
- **Texture**: subtle mid-century printed paper grain, slight
  off-register color registration.
- **Typography**: TBD. Geometric sans (Futura-family) is the
  period-correct default; revisit when scaffolding.

### Image generation prompt template

DALL-E rejects prompts that name Miroslav Šašek directly.
Describe his vocabulary instead.

```
Create a wide landscape illustration (16:9 aspect ratio) in the
style of 1960s mid-century children's book illustration. The
scene: [SCENE DESCRIPTION]. Flat saturated mid-century color
palette — turquoise, coral, mustard yellow, cream — with visible
black ink outlines defining all shapes. Subtle paper grain and
slight off-register color registration, as if printed in a 1960s
children's book. [COMPOSITION DETAILS]. No characters. No text in
the image.
```

Iterate per image. Discuss each before generating.

### Cover image (in flight)

Trylon-and-Perisphere as the iconic anchor — the textbook "World
of Tomorrow" image. Working prompt:

```
Create a wide landscape illustration (16:9 aspect ratio) in the
style of 1960s mid-century children's book illustration. The
scene: an optimistic atomic-age "World of Tomorrow" vista. In the
center, a tall slender white spire and a perfect white sphere
(the 1939 New York World's Fair Trylon and Perisphere). Around
them: a monorail curving past on slender pylons, small jet planes
leaving curved white contrails, and sleek atomic-age buildings
with chrome curves, geometric domes, and antennae in the
background. Flat saturated mid-century color palette — turquoise
sky, coral, mustard yellow, cream — with visible black ink
outlines defining all shapes. Subtle paper grain and slight
off-register color registration, as if printed in a 1960s
children's book. Generous sky at the top, simple ground at the
bottom. No characters. No text in the image.
```

Awaiting first render to react.

## Iconography

Deferred until the visual spine is locked via the cover render.
Open question: how to represent cowns, behaviours, the `when`
keyword, dependency graphs, and the multi-table DB within the
World-of-Tomorrow vocabulary.

## Open questions

- **Talk length**: content first, length follows. Will become
  apparent once the outline is fleshed out.
- **Venue**: Papers We Love San Francisco. Filed under `pwl-sf/`.
- **Typography**: geometric sans assumed; revisit when scaffolding.

## Repo conventions noted

- Reveal.js 5.1.0 from CDN (matches `static/presentations/gc-made-fast/nyc-systems/`)
- `index.html` + `css/custom.css` + `images/`
- Speaker notes via `<aside class="notes">`
- Hugo content page at `content/talks/<slug>/index.md` (not yet
  created)
