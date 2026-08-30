> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# `<cast>` — Characters

**OVML Standard 2.2**

1. Purpose

The `<cast>` element defines the characters, narrators, and voices used by an OVML project.

A character represents a named entity that may be referenced by script content through the `char` attribute of a `<line>` element.

The `<cast>` element groups all character definitions of the project in one document-level container.

The `<cast>` element is optional for projects that do not require character definitions.

2. Structure

The basic structure is:

```text
<cast>

    <character id="vestfal" name="Vestfal" />

    <character id="narrator" name="Narrator" />

</cast>
```

The `<character>` element is the unit of the cast.

The full character reference — required and optional attributes, child elements, voice configuration, and processing preset references — is documented separately.

See: reference/character.md

3. Character Grouping

The `<cast>` element groups all characters and narrators of the project in a single document-level container.

Characters MAY be declared in any order.

Each `<character>` MUST have a unique id within the OVML document.

Script content references a character by its id:

```xml
<line char="vestfal">
    — Это ракеты.
</line>
```

The value of line/@char MUST correspond to an existing character/@id.

4. Characters in Script Context

The script may contain paragraphs with lines assigned to different characters.

Example (detailed in reference/character.md):

```xml
<p>

    <line char="vestfal">
        — Это ракеты,
    </line>

    <line char="narrator">
        — раздался в моей голове чуть насмешливый голос моего спутника —
        дракона Вестфаля.
    </line>

</p>
```

The char attribute identifies the character or voice assigned to the individual line.

A `<p>` MAY contain multiple `<line>` elements assigned to different characters.

5. Scene Participation

A scene MAY list the characters participating in that scene.

Each `<char>` entry references a character id declared in the `<cast>` element:

```xml
<scene>

    <characters>
        <char ref="vestfal" emotion="thoughtful" />
        <char ref="narrator" emotion="neutral" />
    </characters>

</scene>
```

Scene participation references the characters of the cast.

It does not define new characters.

See: reference/scene.md

6. Pronunciation Control

Pronunciation guidance for spoken content lives in the voice and pronunciation model.

Word-level pronunciation control is provided by the `<w>` element:

```xml
<p char="narrator">
    Организация <w alias="Эф Би Ай">FBI</w> провела расследование.
</p>
```

The `<w>` element may set explicit stress, provide abbreviation aliases, supply a phonetic transcription, or ignore a word during synthesis.

See: reference/voice.md

7. Processing Presets

A project MAY apply processing to the voices and media of its characters through processing presets.

Processing presets are referenced from character configuration:

```xml
<character
    id="vestfal"
    name="Vestfal"
    audioProcessorId="preset_1"
    audioProcessorName="Warm Voice"
    audioProcessorFile="presets/audio/WarmVoice.ovml" />
```

The reference mechanism is documented in reference/character.md.

The processing element formats are documented in the processing references.

See: reference/character.md
See: reference/audio-processing.md
See: reference/video-processing.md
See: reference/image-processing.md

8. Design Principle

The `<cast>` element describes who the project's characters are, how their voices are requested, and which processing presets are associated with them.

It does not prescribe a particular TTS provider or playback implementation.

Character defines who the speaker is. Voice configuration defines how the character's voice is requested. Line-level properties define how the character speaks a particular line.

This separation allows the same OVML document to be used by different authoring tools, Players, TTS engines, operating systems, and rendering environments.

9. Related Documents

See: reference/character.md
See: reference/voice.md
See: reference/emotions.md
See: reference/scene.md
See: reference/audio-processing.md
See: reference/video-processing.md
See: reference/image-processing.md