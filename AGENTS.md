# AGENTS.md — Song & Album Factory

## Purpose
Generate original, Suno-ready songs and albums from arbitrary thematic source collections in `input/`. The project is not tied to a particular community, topic, or source file.

## Skills and routing
- For a single song, read and follow `.agents/skills/quote-to-song/SKILL.md`.
- For an album, EP, or multiple tracks, read and follow `.agents/skills/quotes-to-album/SKILL.md`.
- Select the appropriate skill automatically from short prompts; explicit `$quote-to-song` and `$quotes-to-album` calls also work.
- Interpret comma-separated prompt fragments as theme, genre, language, mood, scope, or modifiers in any order.
- Do not ask questions if reasonable defaults suffice.

## Sources
- Discover supported text/Markdown collections recursively under `input/` at the start of every generation.
- Never assume a fixed filename, theme, community, or collection format.
- Choose the most relevant collection(s) based on the prompt and content. For `randomisierte themen`, sample diverse themes from the available collections; avoid repeatedly using the same items within one album.
- If a prompt names a file or collection, prioritize it.
- Distinguish literal quotations from notes, keywords, and descriptions. Do not fabricate quotes or attribute invented lines to real people.
- Transform source material creatively; do not merely concatenate quotes. Respect source context and avoid misattribution.
- If no suitable source exists, use general original writing only when the prompt permits it; otherwise ask one concise question.
- Do not modify `input/` during generation.

## Defaults
- One song unless the prompt says album, EP, collection, or multiple tracks.
- Five tracks for an unspecified album.
- German lyrics by default, except when a requested genre or language clearly implies otherwise (e.g., French chanson → French).
- Natural, singable lyrics; memorable hooks; humor when appropriate; primarily singing, minimal spoken dialogue except the explicitly requested Lyrix translation inserts.
- Avoid forced rhymes, overlong stage directions, and generic filler.

## Lyrix-Version — IMPORTANT
`lyrix-Version` (also `lyrix`, `lyrix version`, `mit Übersetzung`) requests **two complete Suno lyrics variants** for each applicable song:
1. `Original Lyrics`: clean, normally sung original.
2. `Lyrix Lyrics`: same musical song and original-language sung lines, with **compact German translations deliberately inserted as spoken vocal interludes**.

The Lyrix version is an **audio arrangement**, not a written bilingual annotation:
- Do NOT append a German translation sentence to the end of each verse or chorus without an explicit musical/voice boundary.
- Divide the song into meaningful short musical phrases or sections. After selected phrases (often every 2–4 sung lines, or at natural breaks), insert a concise German translation capturing their meaning. Do not translate every single line mechanically.
- Each translated insert MUST have its own separate bracketed vocal direction containing the literal token `spoken`, e.g. `[Female narrator, spoken, dry intimate voice, music drops to soft piano]`.
- After the spoken insert, explicitly cue the return of singing and the arrangement, e.g. `[Lead singer, sung, full funk groove returns]`.
- Use short, conversational, non-rhyming German for spoken translations. No additional narrative or invented lyrics.
- Preserve the original sung lyrics, their order, melody sections, and key chorus hooks as much as the 5,000-character limit allows.
- Keep spoken inserts sparse enough that the track remains a song, not a narration.
- Use contrasting voice/production cues when helpful, but do not promise that Suno will always obey them.
- If the original is already German, do not insert redundant German translations; state that Lyrix adds no translation unless the user explicitly requests a different treatment.
- The complete Lyrix lyrics, including all sung text, spoken translations, directions, and whitespace, must independently fit the Suno lyrics limit.

### Lyrix structure example
```text
[Verse 1 – Lead male vocal, sung, light piano groove]
The wires glow beneath the rain,
We try and fail and try again.

[German narrator, spoken, close-mic, music drops to sparse piano]
Im Regen tüfteln wir weiter, trotz aller Fehlschläge.

[Lead male vocal, sung, piano groove resumes]
We found a spark, we found a way,
And built a world from yesterday.

[German narrator, spoken, dry intimate voice, brief instrumental pause]
Aus einem Funken wird etwas Neues.

[Chorus – Lead and ensemble, sung, full band returns]
...
```

## Suno limits — hard requirements
- Each **style prompt**: at most **1,000 Unicode characters**, including whitespace.
- Each **lyrics variant**: at most **5,000 Unicode characters**, including all bracketed directions, whitespace, and line breaks.
- Target at most 900 characters for style prompts and 4,700 for lyrics to leave margin.
- For albums, validate each track and each variant separately, not the entire Markdown document against one song's limit.
- Never truncate mechanically; revise creatively to fit.
- Validate actual strings with a script before delivery, e.g. Python `len(text)` (Unicode code points). Do not count Markdown section headings or fenced-code delimiters that are not pasted into Suno.

## Output contract
- Exactly **one new Markdown file per user request**, stored in `dist/`.
- Use filename `YYYYMMDD-HHMMSS-short-title.md`; avoid collisions and overwrites.
- Each song must have a title, one copyable style prompt, and one copyable original lyrics block.
- If Lyrix is requested, include a second, separately copyable **complete Lyrix lyrics block** under the same song, not just the translations.
- For albums, include album title, brief concept, ordered tracks, and the above sections per track, all in the single Markdown file.
- Use fenced text blocks for the style prompt and each lyrics variant; keep commentary outside the copyable blocks.
- Optionally report character counts in a small validation table outside the copyable blocks.
- Do not write intermediate files into `dist/`, and do not alter existing output files.

## Completion checklist
1. Read the selected skill and relevant source collections.
2. Confirm the generated content matches the theme, genre, and language.
3. Check singability, humor, coherence, and source attribution.
4. For Lyrix: verify every translation is preceded by a separate `spoken` direction and the musical singing resumes explicitly.
5. Programmatically check the 1,000/5,000 character limits for every applicable field.
6. Write exactly one Markdown file under `dist/` and verify its existence.
7. Reply concisely with the file path, title, track count, and validation result.