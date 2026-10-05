# Writing an interactive episode

Selected writing guidance adapted from the creator's *Making stories that work on StoryKernel* (provided September 2026), grounded in the Whispers experiments. Use this when composing a blueprint. The current owner `build_schema`, template purposes, and generation guidance define the executable contract. The guide's twenty-question interview, rendered proportion, and mystery-specific cast are not StoryBlueprint requirements. New version-2 drafts use the current template's length bounds: 8-15 blocks across the entire playable episode path, at most 3 blocks per scene, and an estimated 60-90 seconds per block. Alternative branches are alternatives, not extra runtime. A scene shares one place and cast throughout its blocks; a change starts another scene. A new scene in the same place reuses the same location name -- that is the common case, not the exception. Declare a new location only when the story moves somewhere else, and a state only when something inside one place visibly changes.

## Scenes worth playing

Give each character a want tonight and private pressure independent of the central answer. A wrongly accused person still has something worth revealing. In microdrama, an audience mistake can expose a vulnerability or impose a relationship cost without turning the conflict into a police case.

Keep each character's `role` to a short label of at most 50 characters. Write `backstory` as a developed show-character biography: aim for 4-6 substantial paragraphs, never fewer than 3. Develop formative history, relationships, social position, worldview, contradictions, behavior, vulnerability, and possible growth beyond the immediate plot. Sparse creator input is permission to invent consistent connective detail, not a reason to deliver a thin biography. Preserve established facts and record consequential additions as assumptions.

Start every character's `backstory` with a fairly verbose physical description: one or two substantial paragraphs at the very top, before history, relationships, or motivations. Describe apparent age, height and build, skin tone, face and eyes, hair, distinguishing features, typical clothing and accessories, posture, and movement in concrete visual prose that makes the character recognizable. A brief adjective list is not enough. Follow this opening with the developed biography; appearance must not crowd out its depth. Preserve the creator's established appearance details and invent consistent missing details, recording consequential additions as assumptions.

Give each distinct worldbuilding concept its own `blueprint.world_information` entry with a specific `label` and detailed `content`. Develop the relevant places, institutions, customs, history, beliefs, technology, and social pressures in paragraphs that explain their consequences. Avoid a generic "World and tone" catch-all. Tone remains separate. Prefer a useful, developed account over a checklist of shallow labels.

For hidden state, design observable tells that can be spoken before the answer is revealed. Make each possibility plausible. Do not let the first suggestive clue become an instant confession; preserve pressure, denial, and eventual change. Hidden state is optional where it does not serve the template.

Audience influence needs a fiction: someone hears it, interprets it, can resist it, and need not announce its source. Show an early consequence; reuse the same early choice later; have a character acknowledge the resulting help or harm. A converging event can bring everyone to the same destination while their reasons and relationships remain changed. Plant that event before it arrives. Maintain a personal emotional outcome where it gives the audience a second meaningful way to succeed.

## Beat ledgers

- Start each block direction with a short action summary of 2-4 sentences, third-person present, with the acting or speaking character named. Put supporting constraints and context below that summary. Describe intent: “Mara challenges the missing hour in his story,” rather than “Mara is suspicious.”
- Write reactions to known stimuli. Reserve an open reaction instruction for genuinely unpredictable audience material.
- Give each live beat a named-character exit line or exit action that leads into the next moment. Exact dialogue is useful for stings and critical facts, not as a script for every exchange.
- Place scoped audience guidance before the affected action. State who is influenced, what may change, how the other person responds, and what remains concealed.
- Give suggestions believable resistance over an exchange. Do not promise instant success or make characters announce a tactic, judge, or hidden mechanism.
- Place a specific “do not reveal until…” constraint at the moment premature revelation would break the scene. Keep necessary constraints in the actual beat direction or character material the runtime uses.
- Use line budgets for connective moments when helpful; let an important confrontation breathe. Recheck all character names when adapting similar beats.

Scene cast is explicit and independent of renderer models. Every character who speaks or acts in a block belongs to that scene's cast. Every listed scene character participates somewhere in its blocks. Background references to absent people belong in context, not the action summary. The compiler preserves this roster for the editor and playback checks; never substitute spawn commands or a prose heading for it.

## A memory of the performance

Specify what the runtime should retrieve from a completed earlier dialogue and where it is recalled. For example, capture the particular excuse a character improvised under pressure, then have someone later challenge that excuse in their own words. The later beat must consume the captured result and react to it. Do not supply the supposedly improvised answer yourself.

A memory query should describe observable dialogue evidence, without author-only reasoning that a character might accidentally say aloud. Its fallback must be a coherent line or action in this show's voice when no matching dialogue exists. The callback should still make sense when the audience is quiet.

## Reusable writing patterns

Adapt these patterns to the exact schema; braces here describe creative placeholders, not engine syntax.

**Audience guidance:** “{Lead} draws on the audience's concrete suggestion while pressing {other character}. Act the approach out; do not announce the tactic or explain its source. {Other character} resists in their own voice before the exchange changes them. Preserve the scene's essential revelation.”

**Free-text direction, when supported:** “Synthesize a short directive from the audience's specific tactical ideas, imagery, and memorable phrasing. Preserve a sharp unusual suggestion rather than averaging it into a generic mood. Do not merely return one message verbatim.”

**Character-grounded judgment, when supported:** “Decide whether {character} takes {action}, using their vulnerability and the substance of the whole exchange. A burst of audience sentiment alone does not overturn their motivation. If {ceiling condition}, choose {bounded outcome}; when ambiguous, use {honest default}.”

**Quiet-room fallback:** “The {audience channel} is quiet. {Lead} follows the concrete course that fits their character.”

**Productive wrong turn:** “{Character} defends their account, gives up the separate secret under pressure, and points the lead toward a named new person or concrete piece of evidence.”

These are creative heuristics, not claims that every renderer or runtime currently exposes each feature. Use only capabilities represented in the returned owner contract. In particular, let the compiler validate opening/closing order, reference consumption, branch coverage, and fallbacks instead of inferring timing from historical Whispers behavior.
