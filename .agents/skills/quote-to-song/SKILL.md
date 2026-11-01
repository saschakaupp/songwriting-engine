---
name: quote-to-song
description: Generate complete Suno-ready songs from arbitrary thematic collections and short prompts, including optional Lyrix versions with sung originals and spoken German translations.
---

# Quote-to-Song

## Purpose

Create original songs from a collection of quotations. Treat quotes as evidence of a shared world, not as a checklist of punchlines. Discover recurring themes, humor, relationships, conflicts, values, and emotional subtext, then transform these insights into memorable lyrics with musical direction.

Be independent of any particular organization, fandom, community, person, or scenario. Never import names, running jokes, or anecdotes from previous projects unless the current user supplies or explicitly requests them.

## Inputs

Accept any combination of:
- A raw quote collection (text, files, chat excerpts, notes, screenshots if readable).
- Background on the speakers, community, or setting.
- Desired subject, message, mood, genre, language, duration, and target music generator.
- Examples of previously successful or unsuccessful songs.
- Output constraints such as a maximum character count.

Start with the material available. Ask only when a missing detail would materially block the task; otherwise choose creative defaults. When invoked from a project with AGENTS.md, follow its source selection, output path, naming, and validation rules. A short prompt is an instruction to generate a finished song, not to propose concepts.

## Workflow

### 1. Parse and understand the collection

Identify distinct quotations and, where available, their speakers and context. Preserve exact wording internally; do not confuse an original quotation with an interpretation. If the source is incomplete, mark the context as unknown rather than inventing a backstory.

For each useful quote, assess:
- **Literal meaning:** What is actually being said?
- **Possible subtext:** What does it suggest about shared habits, values, tension, or relationships?
- **Comic mechanism:** Absurdity, understatement, hyperbole, technical jargon, wordplay, incongruity, reversal, etc.
- **Musical potential:** Hook, call-and-response, narrative incident, verse detail, bridge revelation, or outro callback.
- **Accessibility:** Will outsiders understand it? Can the lyric supply context naturally?
- **Sensitivity:** Does it involve private information, an identifiable individual, protected traits, trauma, or a joke that might humiliate its subject?

Distinguish confident interpretations from speculative ones. Do not diagnose personalities or attribute private motives based on jokes.

### 2. Build a thematic map

Cluster the collection into a few useful themes: e.g., technical optimism, everyday mishaps, rituals, food, shared language, mutual support, creative chaos, or the tension between grand ambitions and trivial outcomes. These are examples, not predefined categories.

Identify recurring contrasts that could drive a song, such as:
- Expert knowledge versus ridiculous outcomes.
- Serious language applied to mundane events.
- Individual quirks versus collective belonging.
- Outward chaos versus inner reliability.
- Playfulness versus a sincere emotional core.

Form a one-sentence **song thesis**: what is the song really about? This should guide the choice of quotations, narrative progression, and refrain.

### 3. Curate quotations deliberately

Choose quotes that support the thesis, fit the rhythm, and complement one another. Do not simply insert the funniest lines in sequence.

Maintain variety across a multi-song project. Prefer underused quotes when earlier songs repeatedly relied on the same ones. Track usage within the available conversation or supplied catalog; never claim to have a complete history if it is unavailable.

Use quotes in three ways:
1. **Verbatim** when the wording is intrinsically musical or important.
2. **Adapted** when grammar, rhyme, meter, or translation requires it.
3. **Alluded to** when a direct quote would interrupt the song.

Never present an adapted or invented line as an exact quotation. Preserve the essential meaning of quotes unless the user explicitly requests surreal reinterpretation.

### 4. Design the musical concept before writing

Choose a genre and arrangement that enhance the material, rather than defaulting to the same sound every time. Define:
- Working title and language.
- Approximate BPM, groove, energy, and instrumentation.
- Vocal character, number of voices, and delivery.
- Narrative viewpoint and emotional trajectory.
- Main hook and chorus concept.
- Section structure and expected length.

Consider surprising genre/theme pairings when appropriate: a dramatic ballad about a small mishap, dance-pop about collective rituals, or cabaret about procedural absurdity.

If the user references a living artist, translate the request into broad musical attributes (groove, production, instrumentation, vocal dynamics, genre) rather than reproducing a recognizable artist's signature style or lyrics.

### 5. Write a coherent song

A useful default structure is:
`[Intro] → [Verse 1] → [Pre-Chorus] → [Chorus] → [Verse 2] → [Chorus] → [Bridge] → [Final Chorus] → [Outro]`

Vary structure when the genre demands it. A song should have progression:
- **Opening:** Establish the setting and invite curiosity.
- **Verses:** Introduce specific details, anecdotes, and escalating connections.
- **Chorus:** Express one strong, repeatable idea, not a catalog of references.
- **Bridge:** Add a meaningful reversal, vulnerability, or new perspective when appropriate; do not force sentimentality.
- **Ending:** Pay off a motif, return to an earlier image, or leave a memorable final line.

Favor vivid concrete images, internal rhythm, natural phrasing, and hooks that are easy to sing. Avoid clumsy rhyme, forced syntax, repetitive filler, and exposition that sounds like documentation. The listener should enjoy the song even without recognizing the source quotes.

### 6. Prepare generator-ready output

For Suno requests, produce two clearly separated Markdown sections in the single output artifact:

**Lyrics**
- Include clear section markers such as `[Verse]`, `[Chorus]`, `[Bridge]`.
- Use concise English production cues in brackets if the generator accepts them.
- Keep sung words separate from stage directions.
- Do not add spoken sections by default. Spoken narration is opt-in; some generators turn it into shouting or exaggerated announcements.
- If a requested spoken segment is intentional, specify voice, language, delivery, and instrumental backing explicitly.

**Style prompt**
- Write a concise English production description covering genre, BPM, groove, instrumentation, vocal character, mood, dynamics, and key exclusions.
- Describe the sound concretely rather than relying on an artist's name.
- Avoid contradicting the lyric structure (e.g., "no speech" while the lyrics request a narrator).

For Suno, lyrics including section markers and all whitespace must be at most 5,000 Unicode characters; style prompt at most 1,000. Aim for <=4,700 and <=900 respectively. Count with a script before delivery. For Lyrix mode, count original and Lyrix lyrics independently. If over limit, rewrite musically rather than truncating mechanically.

### 7. Multilingual songs and translations

Write idiomatic lyrics in the requested language. Prioritize natural syntax, singability, stress, and cultural fit over literal word-for-word conversion. Check that jokes and metaphors survive translation; adapt them when necessary without changing their core meaning.

For a requested **translation**, preserve the original song's meaning, structure, and tone; do not silently rewrite the story. Label a freer singable adaptation as an adaptation.

### Lyrix version — spoken radio translations

When the user requests `lyrix-Version`, create a second complete lyrics arrangement using the proven radio-translation format.

**Canonical structure:**

```text
[Verse – French male singing]
Original-language lyrics, normally four lines.

[Instrumental – German male radio voice, calm spoken translation]
A concise, natural German translation of the preceding passage.

[Verse – French male singing]
Next original-language passage.

[Instrumental – German male radio voice, dry delivery]
A concise German translation of that passage.
```

**Rules:**

- Keep the original-language lyrics sung.
- Insert a separate German spoken translation immediately after each meaningful musical passage, normally a verse, chorus segment, pre-chorus or bridge.
- Use a **single bracketed instruction** beginning with `[Instrumental – German ... radio voice, ...]` for each translation.
- The German text is spoken, never sung.
- Translate the meaning of the immediately preceding passage, concisely and idiomatically. Preserve the humor, punchline and important details.
- The translation may summarize four sung lines in one or two natural German sentences.
- Keep the original song structure, but split longer verses or choruses into shorter, self-contained musical passages where useful.
- Prefer a calm, dry, understated radio delivery. Adjust the emotional tone where appropriate.
- Do not introduce separate `[German narrator]`, `[Instrumental break]`, `[Full groove returns]` or `[spoken response]` tags by default.
- Do not turn the translation into a dialogue, rhythmic spoken vocal or additional sung part.
- Use the same pattern regardless of whether the original language is French, English, Japanese or another non-German language.
- For German originals, do not add redundant German translations unless explicitly requested.
- Respect the 5,000-character lyrics limit for each complete version, including all tags and translations.

**The objective:** A song in its original language, periodically interrupted by a distinct German radio-style voice that translates the preceding sung passage. The translations should sound like brief radio commentary between musical sections, not like an additional character performing in the song.