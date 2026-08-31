# OVML Reference

**OpenVML — Open Voice Markup Language** is an open, XML-based standard for describing structured
audiovisual content.

This section is the normative-oriented reference for the OVML 2.2 document model.

While the [`concepts/`](../concepts/) section explains why OVML works the way it does, the Reference
section describes **the actual elements, attributes, values, and relationships used in an OVML
document**.

## Document Model

An OVML document has the following high-level structure:

```xml
<ovml version="2.2" lang="en">
    <meta>
        ...
    </meta>

    <cast>
        ...
    </cast>

    <assets>
        ...
    </assets>

    <world>
        ...
    </world>

    <script>
        ...
    </script>
</ovml>
```

The main document sections are:

| Element       | Purpose                                            |
| ------------- | -------------------------------------------------- |
| `<ovml>`      | Root document element                              |
| `<meta>`      | Project metadata and presentation preferences      |
| `<cast>`      | Characters, voices, and character-related settings |
| `<assets>`    | Media resource definitions                         |
| `<world>`     | World canon — canonical entities of the project    |
| `<script>`    | Main audiovisual content                           |

## Reference Map

### Document

[`document.md`](document.md)

Defines:

* the root `<ovml>` element;
* document hierarchy;
* required and optional sections;
* document-level attributes;
* structural relationships.

### Metadata

[`meta.md`](meta.md)

Defines:

* project title;
* author;
* presentation preferences;
* subtitle preferences;
* metadata attributes.

### Cast

[`cast.md`](cast.md)

Defines the `<cast>` container and its role in grouping character definitions.

### Character

[`character.md`](character.md)

Defines the `<character>` element, including:

* character identity;
* aliases;
* gender;
* age;
* narrative role;
* color;
* personality;
* backstory;
* voice information;
* pitch and rate;
* optional processing preset references.

### Script

[`body.md`](body.md)

Defines the `<script>` content model and the hierarchy used to organize audiovisual content.

### Chapter

[`chapter.md`](chapter.md)

Defines the `<chapter>` element — a logical division of the script containing scenes and blocks.

### Scene

[`scene.md`](scene.md)

Defines the `<scene>` element and its attributes, including:

* `color`;
* `atmosphere`;
* `transition`.

Scenes provide narrative and visual context for groups of content. The `<camera>` child element is
documented here.

### Lines

[`line.md`](line.md)

Defines spoken and textual lines, including:

* character association;
* speech;
* timing;
* text formatting;
* word-by-word presentation;
* marquee behavior;
* grid positioning;
* transitions;
* keyframes.

### Paragraph

[`paragraph.md`](paragraph.md)

Defines the `<p>` block element — a container for lines that groups narration or dialogue.

### Media

[`media.md`](media.md)

Defines audiovisual media blocks:

* `<video>`;
* `<audio>`;
* `<img>`.

It describes their common timing, positioning, layering, sizing, and playback attributes.

### Subtitle

[`subtitle.md`](subtitle.md)

Defines the `<subtitle>` element for subtitles/captions in meta preferences and media.

### Break

[`break.md`](break.md)

Defines the `<break>` element used to introduce an explicit pause in the content timeline.

### Timing

[`timing.md`](timing.md)

Defines the OVML temporal model, including:

* `startMode`;
* absolute timing;
* relative timing;
* delays;
* durations;
* overlapping content;
* simultaneous background and foreground media;
* synchronization of speech and media.

### Layers

[`layers.md`](layers.md)

Defines the layer system (`background`, `foreground`, `overlay`) used for visual composition of
media.

### Blocking

[`blocking.md`](blocking.md)

Defines the `<blocking>` element for semantic character relationships inside scenes.

### Emotions

[`emotions.md`](emotions.md)

Defines the `emotion` and `intonation` attribute vocabularies.

### Transitions

[`transitions.md`](transitions.md)

Defines transition types and their use on scenes and media blocks.

### Assets

[`assets.md`](assets.md)

Defines how OVML identifies and references media resources.

Assets may be packaged inside a project or referenced externally, depending on the project form and
Player capabilities.

### World Canon

[`world.md`](world.md)

Defines the `<world>` element — the canonical entities of a project's world, including:

* free, extensible sections (`<locations>`, `<terms>`, ...);
* the uniform section rule (entities with stable `id` + `ref` reference mechanism);
* scene-specific `<variation>`.

The world canon keeps recurring entities consistent across many scenes and chapters.

### Locations

[`locations.md`](locations.md)

Defines the `<locations>` section and `<location>` entity within `<world>`, and the `<location ref>`
+ `<variation>` mechanism inside `<scene>`.

### Audio Processing

[`audio-processing.md`](audio-processing.md)

Defines the `<audio_processing>` element for declarative audio post-processing.

### Video Processing

[`video-processing.md`](video-processing.md)

Defines the `<video_processing>` element for declarative video processing.

### Image Processing

[`image-processing.md`](image-processing.md)

Defines the `<image_processing>` element for declarative image processing.

### TTS

[`tts.md`](tts.md)

Describes TTS engine/voice resolution, provider references, and the rule that credentials MUST NOT
be stored in OVML.

### Voice

[`voice.md`](voice.md)

Defines voice attributes (pitch, rate, volume, timbre) and SSML pronunciation controls (`<w>`).

### Identifiers

[`identifiers.md`](identifiers.md)

Describes the ID/reference system across the document: stable ids, refs, scopes, and resolution.

### Enums

[`enums.md`](enums.md)

Consolidated vocabulary of all enum attribute values in OVML.

### Validation

[`validation.md`](validation.md)

Describes structural validation, conformance levels, XSD reference, and forward compatibility.

## Content Hierarchy

A typical script may be organized approximately as:

```text
<ovml>
└── <script>
    └── chapter
        └── scene
            ├── p
            │   ├── line
            │   ├── line
            │   └── line
            │
            ├── video
            ├── audio
            ├── img
            ├── break
            └── camera
```

The exact structure and allowed relationships are defined by the individual reference documents.

## Timing Is Independent of Visual Position

OVML separates temporal behavior from visual positioning.

For example, a background video may occupy the entire composition while a spoken line plays over it:

```xml
<video
    src="background"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="30" />

<line
    char="narrator"
    startMode="absolute"
    startTime="2">
    The story begins here.
</line>
```

A foreground media element may then start while the line is still playing:

```xml
<img
    src="character"
    layer="foreground"
    startMode="absolute"
    startTime="4"
    gridRow="2"
    gridCol="3" />
```

The elements do not have to form a single sequential chain.

## Processing Presets

OVML may reference standalone processing preset documents.

For example:

```xml
<character
    id="vestfal"
    name="Вестфаль"
    audioProcessorId="dmitry_low_husky"
    audioProcessorName="Low & Husky (Extreme)"
    audioProcessorFile="presets/audio/dmitry_low_husky.ovml" />
```

Preset definitions are documented separately under:

[`../presets/`](../presets/)

The Standard describes the reference mechanism; authoring applications may create and manage presets
in different ways.

## External Resources

An OVML document may describe content without embedding every resource directly into the XML.

Depending on the project form, resources may be:

* local files;
* files inside an OVMZ package;
* external URLs;
* other permitted resource identifiers.

The OVML document describes **what resource is required and where it is referenced**. The Player is
responsible for resolving and consuming the resource according to its capabilities and security
policy.

## Conformance

An OVML implementation should distinguish between:

1. **Structural validity** — whether the document follows the OVML XML structure and allowed values.
2. **Semantic interpretation** — what a conforming Player should understand from the document.
   **Runtime behavior** — how a particular Player implements playback, buffering, decoding, synthesis,
   rendering, or other platform-specific operations.

A document validator is responsible for structural and syntactic validity. It does not determine
whether a creative decision made by an author or director is desirable.

For example, overlapping speech, video, audio, and images can be completely valid OVML behavior when
their timing relationships are correctly specified.

## Version

This reference describes **OVML 2.2**.

The version is declared by the root element:

```xml
<ovml version="2.2" lang="en">
```

Future versions may extend the language while maintaining explicit version identification.

## Reading the Reference

For implementation work, the recommended reading order is:

1. [`document.md`](document.md)
2. [`meta.md`](meta.md)
3. [`cast.md`](cast.md)
4. [`character.md`](character.md)
5. [`body.md`](body.md)
6. [`chapter.md`](chapter.md)
7. [`scene.md`](scene.md)
8. [`line.md`](line.md)
9. [`paragraph.md`](paragraph.md)
10. [`media.md`](media.md)
11. [`subtitle.md`](subtitle.md)
12. [`assets.md`](assets.md)
13. [`world.md`](world.md)
14. [`locations.md`](locations.md)
15. [`break.md`](break.md)
16. [`timing.md`](timing.md)
17. [`layers.md`](layers.md)
18. [`blocking.md`](blocking.md)
19. [`emotions.md`](emotions.md)
20. [`transitions.md`](transitions.md)
21. [`audio-processing.md`](audio-processing.md)
22. [`video-processing.md`](video-processing.md)
23. [`image-processing.md`](image-processing.md)
24. [`tts.md`](tts.md)
25. [`voice.md`](voice.md)
26. [`identifiers.md`](identifiers.md)
27. [`enums.md`](enums.md)
28. [`validation.md`](validation.md)

For processing presets, continue with [`../presets/README.md`](../presets/README.md).

## OpenVML

OVML is intended to provide an open content description format that is independent of a particular
authoring application or playback implementation.

> **OVML defines what happens and when. The Player determines how it happens on the target platform.**
