> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

OpenVML Timing Model

OpenVML 2.2

## 1. Overview

Timing is one of the fundamental parts of the OpenVML standard.

OVML does not only describe what content exists. It also describes when that content should begin,
how long it should remain active, and how it relates to other content on the timeline.

The timing model is used for:

- dialogue;
- narration;
- subtitles;
- images;
- audio;
- video;
- scene transitions;
- pauses;
- animations;
- camera directions;
- synchronized multimedia content.

The timing model is intentionally independent from the playback technology.

The OVML document describes the intended temporal behavior.

The Player determines how that behavior is implemented on the target platform.

## 2. Core Principle

OpenVML follows the principle:

OVML describes what should happen in time. The Player determines how it is rendered.

For example, an OVML document may specify:

```xml
<line
    char="alex"
    startMode="afterPrevious"
    startDelay="0.5">
    Hello!
</line>
```

The document does not specify:

- which audio API must be used;
- which TTS engine must be used;
- how audio is buffered;
- how the operating system schedules playback;
- which multimedia backend is used.

Those decisions belong to the execution environment.

## 3. Timeline

An OpenVML project can be understood as a timeline containing ordered content.

Conceptually:

```text
Project
  │
  ├── Chapter
  │     │
  │     ├── Scene
  │     │     ├── Block
  │     │     ├── Block
  │     │     └── Block
  │     │
  │     └── Scene
  │
  └── Chapter
```

A block may represent:

- text;
- dialogue;
- image;
- video;
- audio;
- pause;
- other elements defined by the standard or extensions.

Timing determines the relationship between these blocks.

## 4. Time Units

Unless explicitly stated otherwise, temporal values in OVML are expressed in seconds.

Examples:

startTime="5"

means five seconds.

startDelay="0.5"

means a delay of half a second.

duration="10"

means ten seconds.

Fractional values are allowed:

startTime="1.25"

represents one second and 250 milliseconds.

## 5. Block Start Modes

The startMode attribute determines how the start position of a block is calculated.

OpenVML 2.2 defines three primary modes:

afterPrevious
duringCurrent
absolute

### 5.1 afterPrevious

The block starts relative to the completion of the previous relevant textual block.

Example:

```xml
<line char="alex" startMode="afterPrevious">
    Hello, Maria.
</line>

<line char="maria" startMode="afterPrevious">
    Hello, Alex.
</line>
```

Conceptually:

```text
Alex
├──────────────┤
               ↓
             Maria
             ├──────────────┤
```

This mode is useful for:

dialogue;
narration;
sequential text;
audiobook content;
conversational scenes.

### Important semantic rule

afterPrevious is based on the previous textual block, not necessarily the immediately preceding XML
element.

This allows media elements to coexist with dialogue without unintentionally changing the dialogue
sequence.

### 6. duringCurrent

The block starts at a specified time relative to the current temporal context.

Example:

```xml
<video
    src="background.mp4"
    startMode="duringCurrent"
    startTime="3"
    duration="10" />
```

The block becomes active three seconds into the current context.

This mode is useful when an element should be synchronized with another active element.

For example:

```text
Current dialogue
├───────────────────────────────┤
            ↓
            Video
            ├──────────────┤
```

The exact temporal reference is determined by the surrounding playback context.

### 7. absolute

The absolute mode places a block at an explicit position on the timeline.

Example:

```xml
<audio
    src="music.mp3"
    startMode="absolute"
    startTime="0" />
```

Another example:

```xml
<video
    src="intro.mp4"
    startMode="absolute"
    startTime="30"
    duration="15" />
```

Conceptually:

Timeline

```text
0s                  30s          45s
│────────────────────│────────────│
                     ├────────────┤
                       video
```

This mode is particularly useful for:

- background music;
- background video;
- precisely synchronized media;
- presentation elements;
- complex compositions;
- predetermined timelines.

## 8. Default Start Modes

The default depends on the type of content.

For textual blocks such as <line>, the normal sequential behavior is:

afterPrevious

For media blocks such as:

```xml
<audio>
<video>
<img>
```

the default is:

absolute

when no explicit startMode is specified.

Authors are encouraged to specify startMode explicitly when timing is important for readability or
interoperability.

### 9. startTime

startTime specifies a temporal offset according to the selected startMode.

Example:

```xml
<line
    startMode="duringCurrent"
    startTime="2">
    Hello!
</line>
```

The meaning is:

```text
current temporal context
│
├── 0s
├── 1s
├── 2s ──→ line starts
└── ...
```

For:

```xml
<video
    startMode="absolute"
    startTime="15">
```

the media begins at 15 seconds on the relevant timeline.

### 10. startDelay

startDelay adds an additional delay before the block becomes active.

Example:

```xml
<line
    char="alex"
    startMode="afterPrevious"
    startDelay="1">
    Wait...
</line>
```

Conceptually:

```text
Previous block
├──────────────────────┤
                       │
                       │ 1 second
                       │
                       ▼
                    New line
                    ├───────────┤
```

startDelay is additive to the selected start mode.

It does not change the meaning of startTime.

## 11. Duration

Some elements have an explicit duration.

Example:

```xml
<video
    src="background.mp4"
    startTime="0"
    duration="10" />
```

The media is active for ten seconds.

Duration is particularly important for:

- video;
- images;
- background audio;
- visual elements;
- timed effects.

For generated speech, the actual audio duration may be determined by the TTS result.

The Player should use the resolved media duration when the duration is not explicitly defined by the
OVML document.

## 12. Textual Blocks and Speech Duration

A textual block may result in synthesized speech.

For example:

```xml
<line char="alex">
    Hello!
</line>
```

The OVML document does not necessarily contain the resulting audio.

The Player may generate it through a TTS provider.

Therefore:

```text
Text
  ↓
Voice specification
  ↓
TTS
  ↓
Audio
  ↓
Actual duration
```

The actual speech duration may depend on:

- TTS provider;
- selected voice;
- language;
- speech rate;
- pitch;
- pronunciation;
- provider implementation.

The timing model therefore distinguishes between declared timing and resolved media duration.

## 13. Short Speech Blocks

Very short dialogue lines are valid OpenVML content.

For example:

```xml
<line char="alex">
    О!
</line>

<line char="maria">
    Привет!
</line>
```

A Player must not assume that a dialogue block has a minimum duration.

A TTS-generated block may be extremely short.

This is important for:

- realistic dialogue;
- interruptions;
- reactions;
- conversational narration;
- game dialogue;
- animation;
- rapid exchanges.

The Player may use buffering and preloading strategies to ensure that short TTS fragments can be
played without audible gaps.

Such buffering is an implementation detail and is not part of the OVML timing contract.

## 14. Preloading and Timing

The OpenVML timing model does not prescribe a particular buffering strategy.

A Player may preload:

- TTS blocks;
- audio;
- video;
- images;
- other resources.

For example:

```text
Current block
     │
     ├── playback
     │
     ├── preload next block
     ├── preload next + 2
     ├── preload next + 3
     └── preload next + N
```

The value of N is an implementation setting.

It may depend on:

- available memory;
- network speed;
- asset size;
- device capabilities;
- TTS generation speed;
- user configuration.

Preloading MUST NOT change the semantic timing specified by OVML.

## 15. Large Media Resources

OVML does not require a media resource to fit entirely into memory.

Large assets may be streamed by the Player.

For example:

```text
OVML
  │
  ▼
large video asset
  │
  ├── network/disk streaming
  │
  ├── limited buffer
  │
  └── playback
```

The timing model describes when the resource should be active.

The Player determines how the resource is delivered.

This is especially important for:

- large video files;
- long audio recordings;
- high-resolution media;
- remote resources;
- OVMZ containers containing large assets.

## 16. Overlapping Content

OpenVML permits multiple blocks to be active at the same time.

For example:

```xml
<audio
    src="music.mp3"
    startMode="absolute"
    startTime="0"
    duration="60" />

<line
    char="narrator"
    startMode="absolute"
    startTime="5">
    Once upon a time...
</line>
```

Conceptually:

```text
0s                                      60s
│────────────────────────────────────────│
├────────────────────────────────────────┤ music

     ├───────────────┤
     narration
```

This allows OpenVML to describe:

- narration over background music;
- dialogue over video;
- sound effects during dialogue;
- multiple visual layers;
- synchronized multimedia compositions.

The Player is responsible for resolving the actual media playback.

## 17. Layering and Timing

Timing and visual layering are independent concepts.

For example:

```xml
<video
    src="background.mp4"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="30" />

<img
    src="character.png"
    layer="foreground"
    startMode="absolute"
    startTime="5"
    duration="10" />
```

The first element defines when the background video is active.

The second defines when the foreground image is active.

The layer attribute determines visual composition.

Timing determines temporal composition.

## 18. Breaks

A <break> element introduces a pause into sequential content.

Example:

```xml
<break time="1000" />
```

The time value is expressed in milliseconds.

Therefore:

```xml
<break time="500" />
```

represents a 500 ms pause.

A break is useful for:

- dramatic pauses;
- narration;
- dialogue;
- audiobook pacing;
- scene transitions.

A break does not represent media.

It represents temporal spacing in the script.

## 19. Word-by-Word Timing

OpenVML supports word-level text presentation.

Example:

```xml
<line
    wordByWord="true"
    wordByWordMode="single"
    wordDisplayDuration="500">

    <w group="1">Hello</w>
    <w group="1">world</w>
    <w group="2">again</w>

</line>
```

The wordDisplayDuration value is expressed in milliseconds.

Two primary modes are defined:

single

Only the current word is displayed.

Hello

then:

world

then:

again

cumulative

Previously displayed words remain visible.

Hello

then:

Hello world

then:

Hello world again

Word-level presentation is a UI behavior driven by the timing information in the OVML document.

## 20. Timing and Scenes

A scene provides a logical and creative grouping of content.

For example:

```xml
<scene color="#1a1a2e" atmosphere="quiet sunset">

    <line char="narrator">
        The sun was setting.
    </line>

    <video
        src="sunset.mp4"
        startMode="absolute"
        startTime="0"
        duration="10" />

</scene>
```

The scene itself establishes a structural and semantic context.

Its contents still follow the OpenVML timing rules.

Scene semantics are described separately in:

concepts/scenes-and-world.md

## 21. Timing and Camera

Camera instructions may be associated with a scene or other visual context.

Camera timing is subject to the same principle:

The OVML document describes the intended visual behavior; the Player or renderer determines how it
is implemented.

Camera semantics are defined separately in:

concepts/camera.md

## 22. Timing Resolution

Before playback or rendering, the runtime resolves the temporal relationships described by OVML.

Conceptually:

```text
OVML
  │
  ▼
Parser
  │
  ▼
Validator
  │
  ▼
Timeline
  │
  ▼
Timing resolution
  │
  ▼
Playback / Rendering
```

The timing resolver may determine:

- block order;
- start positions;
- end positions;
- dependencies;
- overlaps;
- active media;
- speech timing;
- scene transitions.

The resolved timeline is an internal runtime representation.

It does not need to be serialized back into OVML.

## 23. Timing and Playback

During playback, the runtime maintains a current position on the timeline.

Conceptually:

```text
Timeline
───────────────────────────────────────────→
                         ▲
                         │
                    current position
```

At any point, one or more elements may be active.

The runtime may expose:

- current chapter;
- current scene;
- current block;
- current line;
- current media;
- current position.

These are runtime states and are not themselves additional OVML elements.

## 24. Seeking

A Player may allow the user to seek to another timeline position.

For example:

Before:

```text
0s───────────────20s──────────────40s
                       ▲
                    position
```

After seek:

```text
0s───────────────20s──────────────40s
                                      ▲
                                   position
```

After seeking, the runtime must resolve which elements should be active at the new position.

For example:

- background video may need to start at the appropriate offset;
- audio may need to seek to the corresponding position;
- a scene may become active;
- subtitles may change;
- the current line may change.

Seeking is a runtime operation and does not modify the OVML document.

## 25. Determinism

An OVML document should describe timing deterministically wherever explicit timing is provided.

The same project should produce equivalent temporal relationships across compatible OpenVML
implementations.

However, exact runtime behavior may vary because of:

- TTS generation;
- network latency;
- device performance;
- decoder behavior;
- unavailable resources;
- provider-specific behavior.

Therefore, the standard defines the semantic timing contract, while implementations control
execution details.

## 26. Timing Errors

Invalid timing values are validation errors.

Examples include:

- malformed numeric values;
- invalid startMode;
- invalid negative values where prohibited;
- invalid duration;
- invalid word display duration.

The parser/validator is responsible for validating the syntax and data constraints of the document.

The validator does not judge the creative or directorial quality of timing.

For example, the following may be unusual but is not inherently invalid:

```xml
<line startTime="100">
    Hello.
</line>
```

Whether this produces a desirable creative result is a decision of the author, not the validator.

## 27. Timing and the Director

OpenVML distinguishes between technical validity and creative direction.

The validator ensures that:

XML structure
+
attribute syntax
+
data types
+
required relationships

are valid.

It does not decide whether:

"this scene starts too slowly"

or:

"this character should speak earlier"

is artistically correct.

The author or AI Assistant remains responsible for the creative intent.

## 28. Timing and AI-assisted Authoring

Timing information can also be generated or modified by an AI Assistant.

For example:

```text
Scene
  ↓
Dialogue
  ↓
TTS estimation
  ↓
Timing
  ↓
OVML
```

The Assistant may use timing information to construct a coherent audiovisual sequence.

However, the resulting OVML remains the authoritative project description.

The Player does not need to know whether timing was created manually, by an AI Assistant, or by
another authoring tool.

## 29. Summary

The OpenVML timing model provides a common temporal language for multimedia projects.

The fundamental timing attributes are:

startMode
startTime
startDelay
duration

The primary start modes are:

afterPrevious
duringCurrent
absolute

Additional timing mechanisms include:

```xml
<break>
wordByWord
wordByWordMode
wordDisplayDuration
```

The standard defines temporal intent, not implementation details.

Therefore:

> **OVML defines when content should occur. The Player determines how that content is buffered,
> streamed, synthesized, decoded, rendered, and synchronized on the target platform.**

This separation allows the same timing model to be used for:

- lectures;
- presentations;
- Shorts/Reels;
- audiobooks;
- game voiceover;
- film dubbing;
- anime;
- courses;
- podcasts;
- interactive multimedia projects.

---

## 30. Implementation Independence

The timing model does not require a specific playback engine, operating system, media framework, TTS
provider, or rendering technology.

An OpenVML implementation may use:

- Web Audio;
- HTML5 Media;
- native audio/video APIs;
- hardware-accelerated decoding;
- local TTS engines;
- cloud TTS providers;
- streaming protocols;
- local files;
- remote resources.

These implementation choices must not change the semantic meaning of the OVML timing model.

This allows the same OVML document to be consumed by different OpenVML-compatible applications and
runtimes while preserving its intended temporal structure.
