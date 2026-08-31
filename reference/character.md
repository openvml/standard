> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# `<character>` — Characters

**OVML Standard 2.2**

## 1. Purpose

The `<character>` element defines a character or named voice used by an OVML project.

A character represents a named entity that may be referenced by script content through the `char`
attribute of a `<line>` element.

A character may contain:

- identity information;
- aliases;
- descriptive character information;
- voice configuration;
- voice characteristics;
- processing preset references.

Characters are declared inside the `<cast>` element.

The `<cast>` element is optional for projects that do not require character definitions.

See: [`reference/cast.md`](cast.md)

## 2. Structure

The general structure is:

```xml
    <cast>

        <character
            id="vestfal"
            name="Vestfal"
            gender="male"
            age="adult"
            role="main"
            color="#888888">

            <aliases>
                <alias>Вестфаль</alias>
                <alias>Дракон</alias>
            </aliases>

            <personality>
                Calm, knowledgeable and slightly sarcastic.
            </personality>

            <backstory>
                Vestfal is an ancient dragon accompanying the protagonist.
            </backstory>

            <timbre>
                Deep, warm and slightly rough male voice.
            </timbre>

        </character>

    </cast>
```

Each `<character>` MUST have a unique id within the OVML document.

## 3. Required Attributes

The following attributes are required:

Attribute	Type	Description
id	string	Unique character identifier
name	string	Human-readable character name

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal" />
```

The id is used to reference the character from script content:

```xml
    <line char="vestfal">
        — Это ракеты.
    </line>
```

The value of line/@char MUST correspond to an existing character/@id.

## 4. Character Attributes

The following attributes are optional.

Attribute	Type	Default	Description
color	HEX	#888888	Character color used by authoring tools and user interfaces
gender	enum	neutral	Character gender classification
age	enum	adult	Character age classification
role	enum	minor	Character narrative role

color

Defines the visual color associated with the character.

```xml
    <character
        id="vestfal"
        name="Vestfal"
        color="#4f46e5" />
```

The value MUST use hexadecimal RGB notation:

    #RRGGBB

The color is a presentation hint and does not define the color of generated media.

gender

Defines the character's gender classification.

Allowed values:

Value	Description
male	Male character
female	Female character
neutral	Neutral or unspecified gender

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        gender="male" />
```

This value is descriptive.

The OVML Standard does not prescribe how a Player or TTS engine must interpret gender.

age

Defines the character's age category.

Allowed values:

Value	Description
child	Child
teen	Teenager
young_adult	Young adult
adult	Adult
elderly	Elderly
senior	Senior / pensioner

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        age="adult" />
```

The age category is descriptive metadata.

It does not prescribe a particular voice, pitch, TTS engine, or processing method.

role

Defines the character's narrative role.

Allowed values:

Value	Description
main	Main character
villain	Villain or primary opposing character
minor	Minor character
narrator	Narrator or narrative voice

Example:

```xml
    <character
        id="narrator"
        name="Narrator"
        role="narrator" />
```

The role is descriptive and does not determine playback behavior.

## 5. Aliases

A character MAY contain an `<aliases>` element.

Aliases provide alternative names or references by which a character may be known.

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal">

        <aliases>
            <alias>Вестфаль</alias>
            <alias>Дракон</alias>
            <alias>Спутник</alias>
        </aliases>

    </character>
```

Each `<alias>` contains a text value.

Aliases are primarily intended for authoring tools, search, indexing, and AI-assisted workflows.

Aliases do not create additional characters.

The canonical character identity remains the value of character/@id.

## 6. Personality

A character MAY contain a `<personality>` element.

It describes the character's personality, behavioral traits, temperament, and other characteristics
relevant to the portrayal of the character.

Example:

```xml
    <personality>
        Calm, intelligent and slightly sarcastic.
        Usually hides his emotions behind humor.
    </personality>
```

The content is descriptive.

The OVML Standard does not prescribe how an application must use personality information.

Authoring tools and AI-assisted systems MAY use it when:

- suggesting voices;
- selecting voice presets;
- generating dialogue;
- suggesting emotions;
- selecting media assets;
- assisting with scene direction.

A Player MAY ignore this information during playback.

## 7. Backstory

A character MAY contain a `<backstory>` element.

It contains background information about the character.

Example:

```xml
    <backstory>
        Vestfal is an ancient dragon who has accompanied
        the protagonist for many years.
    </backstory>
```

The backstory may contain arbitrary descriptive text.

It is intended primarily for authoring, editing, indexing, and AI-assisted workflows.

A Player is not required to process or display the backstory during playback.

## 8. Voice Configuration

A character MAY contain voice configuration.

Voice configuration describes the voice requested for the character.

It does not require the consuming application to use a particular TTS implementation.

Attribute	Type	Default	Description
voiceId	string	empty	Provider-specific voice identifier
voiceName	string	empty	Human-readable voice name
voiceLang	string	empty	Language identifier
voiceEngine	string	implementation-defined	TTS/voice engine identifier
pitch	float	1.0	Base pitch multiplier
rate	float	1.0	Base speech-rate multiplier

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        voiceEngine="edge-tts"
        voiceId="en-US-GuyNeural"
        voiceName="Guy"
        voiceLang="en-US"
        pitch="1.0"
        rate="1.0" />
```

voiceId

Identifies the requested voice.

    voiceId="en-US-GuyNeural"

The meaning of voiceId depends on voiceEngine.

The OVML Standard does not define a global namespace for voice identifiers.

Therefore, implementations SHOULD interpret the voice reference as:

    voiceEngine + voiceId

voiceName

Provides a human-readable name for the voice.

    voiceName="Guy"

voiceName is informational and MUST NOT be treated as a unique voice identifier.

voiceLang

Specifies the language associated with the voice.

Examples:

    en-US
    en-GB
    ru-RU
    uk-UA
    de-DE
    fr-FR

Language identifiers SHOULD follow BCP 47 conventions.

The standard does not require an implementation to support every language.

voiceEngine

Identifies the voice or TTS engine associated with voiceId.

Examples may include:

    edge-tts
    elevenlabs
    web-speech
    piper

The standard does not restrict implementations to these values.

Applications MAY support additional engines.

Unknown engine identifiers SHOULD be preserved when an OVML document is read and written.

See: [`reference/voice.md`](voice.md)
See: [`reference/tts.md`](tts.md)

## 9. Timbre

A character MAY contain a `<timbre>` element.

Timbre describes the desired acoustic character of the voice.

Example:

```xml
    <timbre>
        Deep, warm and slightly rough male voice.
    </timbre>
```

Timbre is intentionally represented as descriptive text rather than a fixed enumeration.

This allows the description to be used with different voice systems and AI-assisted workflows.

Examples:

```xml
    <timbre>
        Warm, deep, slightly rough.
    </timbre>
```

or:

```xml
    <timbre>
        Soft female voice with a clear tone and restrained warmth.
    </timbre>
```

The OVML Standard does not require a particular TTS engine to interpret timbre.

A compatible application MAY use the value as a voice-selection or voice-generation hint.

## 10. Pitch

The pitch attribute defines the base pitch requested for the character's voice.

Default:

    1.0

Recommended range:

    0.5 — 2.0

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        pitch="1.1" />
```

The exact interpretation depends on the selected voice engine.

## 11. Rate

The rate attribute defines the character's default speech rate.

Default:

    1.0

Recommended range:

    0.5 — 2.0

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        rate="0.9" />
```

This value represents the character's base speech rate.

A `<line>` MAY specify its own speech rate, which takes precedence for that particular line.

## 12. Voice Presets

An implementation MAY associate a character with a reusable voice preset.

A voice preset is implementation-specific and is not required for a valid OVML document.

If a portable reference is required, an implementation MAY represent it using a provider-specific or
application-specific element.

For example:

```xml
    <voicePreset
        id="preset_21"
        name="Warm Cinematic Male" />
```

The exact structure and semantics of voice presets are implementation-dependent unless explicitly
defined by a future OVML specification.

## 13. Audio Processing

A character MAY reference an audio-processing preset.

Attribute	Type	Description
audioProcessorId	string	Identifier of the audio-processing preset
audioProcessorName	string	Human-readable preset name
audioProcessorFile	string	Path to the preset in an OVMZ container

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        audioProcessorId="preset_1"
        audioProcessorName="Warm Voice"
        audioProcessorFile="presets/audio/WarmVoice.ovml" />
```

audioProcessorFile provides a portable reference when the preset is included in an OVMZ container.

See: [`reference/audio-processing.md`](audio-processing.md)

## 14. Video Processing

A character MAY reference a video-processing preset.

Attribute	Type	Description
videoProcessorId	string	Identifier of the video-processing preset
videoProcessorName	string	Human-readable preset name
videoProcessorFile	string	Path to the preset in an OVMZ container

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        videoProcessorId="video_01"
        videoProcessorName="Cinematic"
        videoProcessorFile="presets/video/Cinematic.ovml" />
```

See: [`reference/video-processing.md`](video-processing.md)

## 15. Image Processing

A character MAY reference an image-processing preset.

Attribute	Type	Description
imageProcessorId	string	Identifier of the image-processing preset
imageProcessorName	string	Human-readable preset name
imageProcessorFile	string	Path to the preset in an OVMZ container

Example:

```xml
    <character
        id="vestfal"
        name="Vestfal"
        imageProcessorId="image_01"
        imageProcessorName="Dark Fantasy"
        imageProcessorFile="presets/image/DarkFantasy.ovml" />
```

See: [`reference/image-processing.md`](image-processing.md)

## 16. Character References in Script

Characters are referenced by their id.

Example:

```xml
    <cast>

        <character
            id="vestfal"
            name="Vestfal"
            gender="male"
            age="adult"
            role="main" />

        <character
            id="narrator"
            name="Narrator"
            gender="neutral"
            age="adult"
            role="narrator" />

    </cast>
```

The script may then contain:

```xml
    <p>

        <line char="vestfal">
            — Это ракеты,
        </line>

        <line char="narrator">
            — раздался в моей голове чуть насмешливый голос моего спутника —
            дракона Вестфаля.
        </line>

        <line char="vestfal">
            — А если точнее, «Гроза», «Гарпия» и несколько «Ос» старой модификации,
        </line>

        <line char="narrator">
            — с нотками знатока добавил он.
        </line>

    </p>
```

The char attribute identifies the character or voice assigned to the individual line.

A `<p>` MAY contain multiple `<line>` elements assigned to different characters.

## 17. Scene Participation

Characters are declared once in the `<cast>` element.

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

Scene participation references existing characters.

It does not define new characters.

See: [`reference/scene.md`](scene.md)

## 18. Character Identity vs. Voice

Character identity and voice identity are separate concepts.

```text
    character
    │
    ├── id
    ├── name
    ├── aliases
    ├── personality
    ├── backstory
    │
    └── voice
        ├── voiceEngine
        ├── voiceId
        ├── voiceName
        ├── voiceLang
        ├── timbre
        ├── pitch
        └── rate
```

Changing a voice does not create a new character.

For example, changing:

    voiceId="voice-A"

to:

    voiceId="voice-B"

does not change the character's identity.

## 19. Unspecified Voices

A character MAY be defined without voice information:

```xml
    <character
        id="alex"
        name="Alex"
        gender="male"
        age="adult"
        role="main" />
```

Such a document is valid.

The OVML Standard does not prescribe a default TTS provider or default voice.

When voice information is absent, the consuming application determines how the character's speech is
resolved.

Similarly, failure to resolve a requested voice is a runtime/provider condition and is not, by
itself, a structural OVML validation error.

## 20. Validation

An OVML validator SHOULD verify:

- character/@id is present;
- character/@name is present;
- character IDs are unique within <cast>;
- gender, when present, contains an allowed value;
- age, when present, contains an allowed value;
- role, when present, contains an allowed value;
- color, when present, uses the required hexadecimal format;
- numeric voice parameters contain valid numeric values;
- character references in script content resolve to existing character IDs.

The validator checks the structural validity of the OVML document.

It does not determine:

- whether a TTS provider is available;
- whether a voice exists at runtime;
- whether an API key is valid;
- whether a voice preset is available;
- whether an audio/video processor is installed;
- whether a particular Player supports a requested voice engine.

These are runtime or implementation concerns.

## 21. Design Principle

The `<character>` element describes who the character is and how the character's voice is intended
to be represented.

It does not prescribe a particular TTS provider or playback implementation.

Character defines who the speaker is. Voice configuration defines how the character's voice is
requested. Line-level properties define how the character speaks a particular line.

This separation allows the same OVML document to be used by different authoring tools, Players, TTS
engines, operating systems, and rendering environments.

## 22. Related Documents

See: [`reference/cast.md`](cast.md)
See: [`reference/voice.md`](voice.md)
See: [`reference/emotions.md`](emotions.md)
See: [`reference/tts.md`](tts.md)
See: [`reference/scene.md`](scene.md)
See: [`reference/audio-processing.md`](audio-processing.md)
See: [`reference/video-processing.md`](video-processing.md)
See: [`reference/image-processing.md`](image-processing.md)
