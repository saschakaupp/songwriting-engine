---
name: quotes-to-album
description: Generate a complete original Suno-ready album from arbitrary themed collections in input/, including coherent concept albums or randomized-theme track collections, using quote-to-song for every track and optional spoken German Lyrix versions.
---

# Quotes-to-Album

## Mission and hierarchy

Create a finished album from the source collections selected by the user's short prompt. Remain independent of any particular organization, fandom, or community. Follow repository `AGENTS.md` for input discovery, prompt parsing, output path, file naming, and mandatory delivery rules. This skill defines album-level creative decisions and quality gates. Explicit user instructions override defaults.

## Dependency

Read `.agents/skills/quote-to-song/SKILL.md` and apply its source interpretation, lyric-writing, Suno prompting, Lyrix arrangement, and quality rules to **each track**. If the file is unavailable, report the limitation rather than claiming it was loaded.

## Inputs and defaults

- Discover and select relevant material from the files under `input/` as directed by `AGENTS.md`. Never assume one particular collection is authoritative.
- Interpret terse prompts autonomously. Example: `metal-album, randomisierte themen` means a five-track metal album drawing varied themes from the available collections.
- Default: **five complete tracks** with an identifiable shared musical identity. The user can request another count.
- A concept album is appropriate when a continuous narrative is requested or emerges naturally from the prompt. For `randomisierte themen`, prefer a **thematically varied collection** with coherent sonic identity, not a forced storyline.
- Default language and other unspecified settings come from `AGENTS.md`.
- Never ask for optional details when a reasonable creative choice is possible.

## Phase 1 — Interpret and curate sources

1. Read the selected source collections, preserving distinctions between exact quotes, context, interpretation, and fiction.
2. Identify themes, comic mechanisms, emotional subtext, recurring images, and usable hooks.
3. Build an **internal quote ledger** recording exact quote, source file, theme, context/sensitivity, assigned track, and whether it is verbatim/adapted/alluded to.
4. Distribute material across tracks; avoid relying on the same few jokes or repeating the same quotes unless an intentional refrain or callback requires it.
5. Never fabricate a quote or attribute an invented or adapted line as verbatim.

## Phase 2 — Choose album structure

### Concept or narrative album

Build an **internal album bible** before drafting:

- Logline, premise, setting, time span, fictionalization boundaries.
- Character or narrator roster where relevant; prefer composite characters when real people are not explicitly authorized.
- Narrative progression, chronology, cause and effect, climax, and payoff.
- Sonic identity, recurring lyrical/production motifs, permitted variations.
- Track map with title, narrative beat, quote references, mood, hook, musical character, and transition.

Every track should meaningfully advance the story or change the listener's understanding. Reprise motifs deliberately; do not expect a music generator to reproduce identical melodies across separate generations.

### Thematically varied or randomized album

When the prompt requests random themes, do **not** force a three-act story. Instead:

- Sample distinct themes from the eligible input collection(s), varying their selection across runs when possible.
- Avoid selecting five close paraphrases of the same theme.
- Build an internal track map with contrasting subject, energy, arrangement, vocal approach, and hook.
- Retain a recognizable album identity through genre, instrumentation, production, or a loose conceptual framing.
- Keep each song independently understandable and satisfying.

For both modes, plan the album internally and **proceed directly to full lyrics**. Do not stop for concept approval unless the user explicitly asks for staged collaboration.

## Phase 3 — Write each track

Apply `quote-to-song` for every track:

1. Define the track's purpose, thesis, source quotes, and emotional trajectory.
2. Choose an appropriate structure, BPM, instrumentation, vocal register, and hook.
3. Write complete, original, idiomatic, singable lyrics with Suno-compatible section tags.
4. Write a separate English Suno style prompt matching the track's arrangement.
5. Preserve coherent album-wide production while varying energy and structure.
6. For narrative albums, check continuity against preceding and following tracks.
7. Update the internal ledger and album bible; do not create separate files for them.

Do not over-specify multiple alternating voices if the arrangement can work with a single vocalist. Spoken sections are opt-in except in an explicitly requested Lyrix version.

## Phase 4 — Lyrix version (optional)

When the prompt includes `lyrix-Version` or an equivalent modifier, create **two versions per track**:

- **Original Lyrics**: the clean, singable song without added German translation narration.
- **Lyrix Lyrics**: an alternate Suno-ready arrangement retaining the original-language sung material, interrupted at musically appropriate points by concise **spoken German translations**.

The Lyrix version is **not** a written line-by-line translation and not merely one German sentence appended to each verse. Integrate several deliberate musical breaks, using a separate narration voice or delivery where appropriate.

Use explicit cues such as:

```text
[Verse 1 – Male lead, sung, full band]
...

[Instrumental break – Drums drop out, soft piano remains]
[Female narrator, German, intimate, spoken]
Kurze sinngemäße deutsche Übersetzung der vorherigen Passage.

[Full band returns]
[Chorus – Male lead, sung]
...
```

- Put `, spoken` in the voice/delivery instruction for each German translation section.
- Make the translation short, accurate, natural, and meaningfully related to the immediately preceding sung passage.
- Mark instrumental withdrawal and return so the translated passage is musically separated.
- Avoid spoken translations after every line; preserve song momentum.
- Include a **separate Lyrix Style Prompt** that explicitly permits German spoken narration and describes the music ducking and resuming. Do not leave contradictory `no spoken words` instructions in that prompt.
- If the original lyrics are German, omit redundant translations and briefly indicate that a German Lyrix translation is not needed unless the user requests a special alternative.
- Suno may still sing spoken passages; these cues improve the odds but cannot guarantee performance.

## Phase 5 — Mandatory per-track limits

For every track and every version:

- **Lyrics <= 5,000 Unicode characters**, counting section tags, spaces, and line breaks.
- **Style prompt <= 1,000 Unicode characters**.
- Aim for <= 4,700 lyric characters and <= 900 style-prompt characters for margin.
- Validate actual lengths programmatically; do not estimate or claim validation without running a check.
- Validate Original and Lyrix Lyrics separately, as well as their respective style prompts.
- Never mechanically truncate; edit for musical coherence and recheck.
- Do not concatenate multiple tracks into one Suno lyrics field.

## Phase 6 — Single-file delivery

Follow `AGENTS.md` for filename and location. **Exactly one Markdown file per album generation, under `dist/`.** Do not write `ALBUM_BIBLE.md`, `QUOTE_LEDGER.md`, `TRACKLIST.md`, individual track files, or production notes as separate artifacts.

Use this structure:

```markdown
# Album title

Brief concept and tracklist.

## Track 01 — Song title

### Style Prompt

[English Suno style prompt]

### Lyrics

[Complete original Suno lyrics]

### Lyrix Style Prompt

[Only when requested]

### Lyrix Lyrics

[Only when requested; complete sung/spoken arrangement]

## Track 02 — Song title
...
```

Include all requested tracks in the same file. Internal planning notes may be condensed into a short album concept, but must not clutter Suno copy sections. Do not claim a partial album is complete. If a requested scope is too large to finish, clearly report that and seek an explicit decision rather than silently splitting the deliverable.

## Phase 7 — Quality gates

Before delivery, verify:

- Source selection matches the user's prompt; no hardcoded collection or setting.
- Album mode matches intent: causal concept album versus diverse randomized collection.
- Tracks have distinct hooks, progression, language quality, and musical identities.
- Quotations are varied, faithful, contextualized, and respectfully handled.
- Narrative continuity holds where a story is intended.
- Every track has complete Lyrics and Style Prompt sections.
- Every requested Lyrix version includes clearly separated spoken German narration and its own compatible style prompt.
- All four applicable per-track character counts are actually validated.
- Exactly one new Markdown file exists in `dist/` for this generation.

Respond concisely with the output path, album title, number of tracks, and verification result.

## Principles

Story before setlist when a story is requested. Variety before forced plot when themes are randomized. Meaning before quote count. Musicality before exposition. Source fidelity before invented backstory. Production constraints before delivery.