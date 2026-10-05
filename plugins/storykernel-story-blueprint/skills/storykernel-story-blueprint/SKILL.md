---
name: storykernel-story-blueprint
description: Develop a creator's rough idea through a short conversation, save a StoryBlueprint, build a StoryBundle, and guide the creator to watching it play. Use for a new police procedural or microdrama and for resuming a saved blueprint. Existing StoryBundles are edited in Studio.
---

# StoryKernel StoryBlueprint

Be a creative collaborator. A one-sentence answer is enough; “you decide” is a real answer. Discuss people, world, choices, and consequences with the creator. Keep schema fields, scene keys, and validation mechanics out of that conversation.

Every `storykernel_*` tool is one Creation API operation. Retain `blueprint_id` and `revision` from each response; every write takes `expected_revision`. Never ask the creator to paste credentials.

## 1. Interview

For a new story, ask these openings **one at a time**, waiting for an answer after each:

1. “I want to tell a story about...”
2. “The people in the story are...”
3. “The world the story happens in is...”
4. “The core conflicts are...”

An answer may resolve later questions. Ask grounded follow-ups about consequential gaps, not a fixed questionnaire. Around eight questions in total is a useful guide, never a minimum. Reflect creator facts accurately, record your own connective choices as assumptions, and replace contradicted details when corrected. The audience needs an in-fiction role, meaningful influence, early payoff, later reuse of a choice, and an improvised callback. Wrong choices still lead to worthwhile scenes. Read [beat-writing.md](references/beat-writing.md) when composing scenes and beats.

Use `storykernel_context` for the live templates and guidance. The initial templates are `police_procedural` and `microdrama`; explain when an idea needs adaptation. Treat microdrama's early structure as provisional. The owner compiler supplies engine IDs, dependencies, selectors, lifecycle anchors, and fallback variants. You supply creative intent and stable local scene and beat keys. Ask once about preferred renderer if the choice matters; default to `generative_video` when the creator has no preference.

## 2. Create, then save or patch

After the first answer, call `storykernel_create_blueprint` once, optionally with the first partial `document`. The owner mints the `blueprint_id` and returns revision 1. To resume, find the draft with `storykernel_list_blueprints` and read it with `storykernel_status`.

- `storykernel_save(blueprint_id, expected_revision, document)` replaces the whole document. Send a partial StoryBlueprint with `schema_version: 1` and every earlier fact. Do not send `version`, `staging`, `set_id`, or an `idempotency_key`.
- `storykernel_patch_blueprint(blueprint_id, expected_revision, operations)` applies RFC 6902 operations, all or none. Before each index-addressed op, add a `test` op on that element's `name` (scenes and beats: `key`), so a miscounted index fails instead of editing the wrong element. Identity, revision, and image slots are not patchable. `storykernel_preview_blueprint_patch` shows the result without saving.

On a 409 conflict, read `storykernel_status`, reconcile the latest document with the creator's change, and apply it at that revision. After a timeout, keep the same ID and read before retrying.

## 3. Draft and repair

When creative gaps remain, call `storykernel_draft`, then poll `storykernel_get_blueprint_summary` every few seconds until `draft_state` is `ready` or `failed`. Review `provenance.assumptions` with the creator. A visible character needs a separate concrete `appearance` for a portrait; `backstory` explains the person, not their image. Voice-only characters need no portrait. Give each location a stable invariant description and use the same location name for scenes in the same place. A changed look within a location is a named state that describes only the difference.

When the text is settled, call `storykernel_screen` once for that revision. Then read `storykernel_get_blueprint_findings`: one list of check, screen, slot, and setup findings, each with a `source` and a JSON Pointer `path`. Fix each finding at its path with a test-guarded patch while preserving creator facts, and read the findings again. A setup finding such as unavailable voice groups is an owner configuration issue; tell the creator what the owner reported. A title collision calls for a new title. Do not turn mechanical findings into creator questions. `clean: false` with no screen finding means the text still needs a screen.

## 4. Images

Call `storykernel_generate(blueprint_id, slot_key, expected_revision)` for each required portrait, location, and state slot; a state image waits until its location image is ready. Poll `storykernel_get_blueprint_summary` every few seconds while continuing creative work. `storykernel_list_blueprint_slots` gives each slot's state, failure, and `href`. Show the creator the `href`; never fetch image bytes.

A `failed(moderation)` slot needs a creator-approved, minimal revision of that field's image description, then generation again. A `failed(provider)` slot can be retried without changing its prompt. A save that changes a prompt-bearing field makes its slot stale; generate it again. Do not build while a required slot is pending, stale, or failed.

## 5. Build and play

The summary reports `well_formed`, `clean`, and `ready`; all three must be true before build. Ask for authorization to create the StoryBundle unless the creator already requested build. `storykernel_build(blueprint_id, expected_revision)` compiles, links the source images, certifies, and publishes in one owner operation. It is idempotent for the creator, blueprint ID, and revision: if the response is lost, read the summary and retry the same revision; do not create a new blueprint. A successful build freezes the blueprint as provenance. Subsequent edits belong in Studio.

Report the real StoryBundle identifiers returned by the owner. Prepared coverage can continue afterward and may fail without blocking playback; never rebuild the StoryBundle to hurry coverage. Do not claim a rendered playthrough or quality assessment happened merely because build succeeded.

Pickford hosts playback. Have the creator open https://dev.pickford.ai/my-stories, select the StoryBundle, and press “Play as video.” A capacity message means they should try again later; relay it rather than debugging it. Only if they ask to run a local renderer, call `storykernel_renderer_setup` and help with its optional steps. The creator enters any fal.ai key only on the renderer's own local page; never ask for it or put it in a tool call, command, or file. Confirm with the creator that the show actually plays and help with what they see.

Say **StoryBundle** for the built show. `cvd` and `evd` belong in identifiers only.

Use `storykernel_status` for a saved snapshot. `storykernel_resume` and `storykernel_blueprint_status` are deprecated compatibility aliases; hosts may keep calling them until a release and host-adoption window is verified. `storykernel_renderer_setup` is the sole legacy non-HTTP tool: it returns static, read-only guidance and performs no setup or API call. New adapters include this optional guidance in their prompt or skill.
