# Song & Album Factory

Generate original, Suno-ready songs and albums from your own collections of quotations, anecdotes, running jokes, and ideas — using Codex and short natural-language prompts.

The repository intentionally starts **without any quotation collections or generated songs**. Bring your own source material: the project supplies the instructions and skills, not a preloaded theme or community.

## Requirements

- [Codex](https://developers.openai.com/codex/) with project-local skill support.
- The repository's `AGENTS.md` and the two skills under `.agents/skills/`.
- Your own quotation collection(s) as Markdown files.
- A music generator such as [Suno](https://suno.com/) if you want to turn the generated lyrics into audio. Codex generates text and style prompts, not audio.

## Start from scratch

Clone or copy the repository and keep just these files:

```text
song-factory/
├── AGENTS.md
└── .agents/
    └── skills/
        ├── quote-to-song/
        │   └── SKILL.md
        └── quotes-to-album/
            └── SKILL.md
```

Create an `input/` directory and add at least one collection:

```bash
mkdir -p input dist
```

```text
song-factory/
├── AGENTS.md
├── .agents/skills/...
├── input/
│   ├── makerspace-zitatesammlung.md
│   └── sci-fi-zitatesammlung.md
└── dist/
```

`input/` holds your source material; `dist/` holds generated Markdown. Both can start empty. Do not put generated output in `input/`.

## Add your own quotation collection

Create any `.md` file in `input/`. There is no mandatory schema. Clear headings and contextual notes help Codex understand jokes without inventing backstories.

Example `input/makerspace-zitatesammlung.md`:

```markdown
# Makerspace — Quotes and anecdotes

## Quotations

- "One more attempt, then we go home." — Said during a late-night repair session.
- "It's not broken; it's expressing an opinion." — About a stubborn device.

## Anecdotes

- A group spent hours fixing a tiny computer. Someone brought dinner,
  and the repair became a social evening.

## Themes

- Friendship, curiosity, shared problem-solving, good-natured chaos.
```

These are illustrative fictional examples. Use your own material, and avoid including private or sensitive details without permission. You can maintain multiple collections, including in subdirectories under `input/`.

## Generate a song

```
flowchart TD
    A["🎵 Codex Command<br/>$quote-to-song friendship, english, lyrix-version"]
    B["📚 Load Quote Collections<br/>from input/"]
    C["🔎 Analyze Quotes<br/>Themes, motifs, and mood"]
    D["🎼 Develop Song Concept<br/>Genre, structure, and language"]
    E["✍️ Generate Original Lyrics"]
    F["🎛️ Generate Original Style Prompt"]
    G{"Lyrix Version<br/>Requested?"}
    H["🎙️ Generate Lyrix Lyrics<br/>Original vocals + spoken<br/>German radio translations"]
    I["🎛️ Generate Lyrix Style Prompt"]
    J["📏 Validate Character Limits<br/>and Completeness"]
    K["📄 Save Single Markdown File<br/>to dist/"]
    L["🎧 Transfer Lyrics and<br/>Style Prompts to Suno"]
    M["🎶 Original Song"]
    N["🎶 Lyrix Version"]

    A --> B --> C --> D
    D --> E
    D --> F
    E --> G
    F --> G
    G -->|Yes| H --> I --> J
    G -->|No| J
    J --> K --> L
    L --> M
    L -->|If generated| N

    classDef input fill:#e0e7ff,stroke:#6366f1,color:#312e81
    classDef process fill:#e0f2fe,stroke:#0284c7,color:#075985
    classDef lyrix fill:#f3e8ff,stroke:#9333ea,color:#6b21a8
    classDef output fill:#dcfce7,stroke:#16a34a,color:#166534

    class A,B input
    class C,D,E,F,G,J process
    class H,I lyrix
    class K,L,M,N output
```

Open the project directory in Codex and enter:

```text
$quote-to-song friendship, community, base song in english, add lyrix version
```

More examples:

```text
$quote-to-song essen, anime-intro
$quote-to-song melancholic piano ballad, source: makerspace
$quote-to-song french chanson, absurd humor, lyrix-version, source: makerspace
```

You can also write a short natural-language request without explicitly naming the skill; `AGENTS.md` tells Codex how to route song and album requests. Explicit `$quote-to-song` is useful for predictable invocation.

The generated Markdown contains a title, a Suno-ready English **style prompt**, and **lyrics** in separately copyable code blocks.

## Generate an album

```text
$quotes-to-album metal-album, randomisierte themen, source: all
```

Or:

```text
$quotes-to-album french chanson, coherent story, source: makerspace
$quotes-to-album electro-swing, 7 tracks, lyrix-version, source: sci-fi
```

The default album length is **five tracks**, unless you request another number. A concept album follows a coherent narrative; `randomisierte themen` permits varied topics while maintaining a recognizable musical identity.

The complete album is saved in **one Markdown file**, with individual style prompts and lyrics for every track. Planning information such as an album bible and quote ledger is used during generation, not emitted as separate files by default.

## Select source collections

Use the optional `source:` parameter:

| Parameter | Meaning |
| --- | --- |
| *(omitted)* | Let Codex choose the most relevant collection from `input/`. |
| `source: makerspace` | Select the matching collection, e.g. `makerspace-zitatesammlung.md`. |
| `source: makerspace,sci-fi` | Use both named collections. |
| `source: all` | Consider every collection under `input/`. |

Names match collection filenames case-insensitively; a suffix such as `-zitatesammlung` may be omitted. For ambiguous or missing matches, clarify the source rather than silently using unrelated material. Without a source, Codex may use general original writing only when your prompt permits it.

## Lyrix version: sung original, spoken German translation

Add `lyrix-version`, `lyrix` or `add lyrix version` to a prompt when the base song is in a language other than German.

The result contains **two complete lyrics variants**:

1. **Original:** The normal sung song.
2. **Lyrix:** The same original-language song, broken into meaningful musical sections, each followed by a **separate, spoken German translation**.

The spoken translation uses a distinct *radio voice*, not an additional singer or an in-song dialogue. The successful format is intentionally simple:

```text
[Verse – French male singing]
Dans notre atelier illuminé,
Et l'on riait tranquillement.

[Instrumental – German male radio voice, calm spoken translation]
In unserer hellen Werkstatt lachten wir ganz entspannt.

[Verse – French male singing]
Puis un ami entra, mystérieux...
```

The translation follows the immediately preceding section and conveys its meaning naturally, often in one or two sentences. Avoid extra `narrator`, `instrumental break`, or `full groove returns` instructions around every translation. The Lyrix version is a separate Suno input, **not** a subtitle file. German originals do not need redundant German translations.

## Output and limits

Each generation creates **exactly one new Markdown file** in `dist/`, with a timestamped descriptive filename. Songs and albums use the same output convention; the project does not generate separate per-track files.

Suno character limits enforced by the project:

| Field | Maximum |
| --- | ---: |
| Style prompt | 1,000 characters |
| Lyrics, original version | 5,000 characters |
| Lyrics, Lyrix version | 5,000 characters |

Limits apply **per track and per variant**, including section tags and whitespace. Codex should count characters programmatically and revise naturally if needed, rather than truncating the output.

To use a result in Suno, copy a track's **style prompt** into the style field and its **lyrics** into the lyrics field. For Lyrix, create a separate generation with the Lyrix lyrics (and its corresponding style prompt, if supplied).

## Project rules and customization

- `AGENTS.md` defines project-wide routing, source discovery, file output, and hard constraints.
- `.agents/skills/quote-to-song/SKILL.md` defines songwriting, quote selection, musical direction, and Lyrix formatting.
- `.agents/skills/quotes-to-album/SKILL.md` builds multi-track albums using `quote-to-song` and manages themes, motifs, and continuity.

To customize behavior, edit these files rather than modifying generated output rules ad hoc. Keep the Lyrix format defined centrally in `quote-to-song` so both skills stay consistent.

If Codex does not recognize a `$` skill invocation, check that the skill files are at the exact paths shown above, contain valid `name` and `description` YAML frontmatter, and that Codex has loaded the project directory. You can also ask Codex to use the named skill explicitly.

## Tips for better results

- Give quotes context: who said them, what happened, and why they were funny.
- Mix direct quotes with anecdotes and themes; a song needs more than a list of punchlines.
- State genre, mood, language, or a source when you have a preference; otherwise let Codex choose.
- For a Lyrix version, expect a longer arrangement because spoken translations take time.
- Review generated lyrics before sharing or publishing, especially when quotes concern real people.

## License and source rights

Only contribute collections you have permission to share. Generated lyrics are intended to be original transformations, not verbatim reproductions of copyrighted songs. Check the terms of your chosen music generator before distributing audio commercially.