> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# OVML Document Structure

OpenVML Standard 2.2

## 1. Purpose

This document describes the overall structure of an OVML 2.2 document and the relationships between
its primary elements.

An OVML document is a declarative audiovisual project script. It describes the project's content,
characters, resources, timing, and directorial instructions.

OVML does not define a specific playback or rendering technology.

OVML defines what should happen and when. The Player or renderer determines how it happens on the
target platform.

## 2. Root Element: <ovml>

Every OVML document has a single root element:

```xml
<ovml version="2.2" lang="en">
    ...
</ovml>
```

### Attributes

Attribute	Type	Required	Description
version	string	Yes	OVML standard version
lang	string	No	Primary language of the document

Example:

```xml
<ovml version="2.2" lang="en">
    ...
</ovml>
```

The value version="2.2" identifies the document as using the model and rules defined by OVML 2.2.

## 3. Overall Structure

A typical OVML document has the following structure:

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

The primary sections have different responsibilities:

```text
<ovml>
│
├── <meta>       Project metadata and preferences
│
├── <cast>       Characters and voice/processing settings
│
├── <assets>     Resources used by the project
│
├── <world>      World canon — canonical entities of the project
│
└── <script>     Script and timeline structure
```

### 4. <meta>

The <meta> element contains information about the project itself.

Example:

```xml
<meta>
    <title>The Last Summer Day</title>
    <author>OpenVML Example</author>

    <preferences
        showSubtitles="true"
        subtitleFontSize="14"
        subtitleBg="rgba(0,0,0,0.7)"
        subtitleColor="#ffffff" />
</meta>
```

Metadata is not part of the script itself.

It describes the project and its general presentation preferences.

See:

reference/meta.md
### 5. <cast>

The <cast> element contains the characters used by the project.

Example:

```xml
<cast>

    <character
        id="alex"
        name="Alex"
        gender="male"
        age="adult"
        role="protagonist"
        voiceId="en-US-GuyNeural"
        voiceName="Guy"
        voiceLang="en-US"
        voiceEngine="edge-tts" />

    <character
        id="maria"
        name="Maria"
        gender="female"
        age="adult"
        role="major"
        voiceId="en-US-JennyNeural"
        voiceName="Jenny"
        voiceLang="en-US"
        voiceEngine="edge-tts" />

</cast>
```

The character ID is referenced by dialogue:

```xml
<line char="alex">
    We have to go.
</line>
```

This avoids duplicating character information in every line.

See:

reference/cast.md
### 6. <assets>

The <assets> element describes resources used by the project.

Resources may include:

- images;
- video;
- audio;
- music;
- sound effects;
- other supported media.

OVML can use referenced external resources.

For example:

```xml
<assets>

    <asset
        id="sunset"
        type="image"
        src="https://example.com/assets/sunset.jpg" />

    <asset
        id="rain"
        type="audio"
        src="https://example.com/assets/rain.mp3" />

</assets>
```

Resources may also be included in an OVMZ container:

```text
project.ovmz
│
├── content.ovml
└── resources/
    ├── images/
    ├── audio/
    └── video/
```

The script can then reference those resources by ID.

See:

reference/assets.md

### 7. <world>

The <world> element describes the canonical entities of the
project's world.

```xml
It is declared once at the top of the document, alongside
<cast> and <assets>.
```

The world canon does not prescribe a fixed set of sections. A document
declares the sections that its content requires.

Every section follows the same uniform rule:

- a section is a container of named entities;
- an entity has a stable id and a display name;
- an entity may contain arbitrary clarifying child elements;
- an entity is referenced from script content by ref.

Example — a story-geared canon:

```xml
<world>

    <locations>

        <location
            id="rusty_anchor"
            name="The Rusty Anchor"
            type="tavern">

            <era>fantasy-medieval</era>
            <style>rough-hewn oak, candlelight</style>
            <palette>hearth:#4a2f1a; walls:#3a2a1e</palette>
            <props>cracked bar, long benches, hearth</props>
            <atmosphere>low murmur, smell of ale and smoke</atmosphere>

        </location>

    </locations>

</world>
```

Example — a knowledge-geared canon (lecture, documentation):

```xml
<world>

    <terms>

        <term
            id="photosynthesis"
            name="Photosynthesis">

            <definition>The process by which light is converted into chemical energy.</definition>

        </term>

    </terms>

</world>
```

The world canon holds the permanent properties of its entities.

Scenes reference a canonical entity by its id rather than
duplicating the full description:

```xml
<scene>

    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>
```

    ...

```xml
</scene>
```

This keeps long-form projects consistent across many scenes and
chapters.

- Scene-specific change is expressed by a <variation> inside the scene,
- never by editing the canon.

The <world> element is optional.

A document without a world canon is valid.

The standard does not define project types. The parser interprets any
section by the uniform rule above, from the document's structure alone.

See:

reference/world.md

### 8. <script>

The <script> element contains the actual project content.

Conceptually:

```text
<script>
│
├── chapter
│   │
│   ├── scene
│   │   ├── camera
│   │   ├── text
│   │   ├── media
│   │   └── ...
│   │
│   └── scene
│
└── chapter
```

Different project types may contain different numbers of chapters and scenes.

## 9. Chapters

A chapter is a logical unit of the script.

It organizes the project into navigable sections and contains scenes and content blocks.

Example:

```xml
<script>

    <chapter id="chapter-1" title="Beginning">

        <scene>
            ...
        </scene>

    </chapter>

</script>
```

The complete chapter model is defined in:

reference/chapter.md

### 10. <scene>

A scene provides the primary directorial context.

It groups elements that occur within the same visual and dramatic context.

Example:

```xml
<scene
    color="#1a1a2e"
    atmosphere="warm sunset, silence, tranquility">
```

    ...

```xml
</scene>
```

The scene model — including its attributes, child elements, location references, camera direction,
and scene boundaries — is defined in:

reference/scene.md

### 11. <camera>

The <camera> element provides a directorial instruction within a scene.

A camera is not a media file.

It describes how the visual content is intended to be presented.

Example:

```xml
<scene>

    <camera
        shot="medium"
        framing="center"
        target="alex"
        movement="static" />
```

    ...

```xml
</scene>
```

The camera model is defined in:

reference/scene.md
concepts/camera.md

## 12. Text Content

The primary element for dialogue and textual content is <line>.

It is commonly contained within a <p> block:

```xml
<p>

    <line char="alex">
        Hello!
    </line>

</p>
```

A <line> may contain:

- dialogue text;
- a character reference;
- timing information;
- display settings;
- word-by-word markup.

Example:

```xml
<line
    char="alex"
    startMode="afterPrevious"
    wordByWord="true"
    wordByWordMode="single"
    wordDisplayDuration="100">
```

    Hello, Maria!

```xml
</line>
```

## 13. Word-by-Word Markup

Text may contain additional word-level markup:

```xml
<line
    wordByWord="true"
    wordByWordMode="cumulative">

    <w group="1">Hello</w>
    <w group="1">world</w>
    <w group="2">how</w>
    <w group="2">are</w>
    <w group="2">you</w>

</line>
```

This allows a Player or renderer to implement different synchronized text-display modes.

For example:

Hello

Hello world

Hello world how are you

or:

```text
Hello → world → how → are → you
```

The exact visual behavior is determined by wordByWordMode and the Player implementation.

## 14. Media Elements

OVML supports different types of media:

```xml
<img src="background" />

<video src="intro-video" />

<audio src="rain" />
```

Media elements may have timing and presentation attributes:

```xml
<video
    src="background-video"
    layer="background"
    volume="1"
    duration="10"
    startMode="absolute"
    startTime="0"
    loop="true" />
```

Media is therefore part of the project's timeline rather than merely being a collection of attached
files.

### 15. <break>

The <break> element represents a pause.

```xml
<break time="1000" />
```

The time value is specified in milliseconds.

Example:

```xml
<line char="alex">
    I wanted to say...
</line>

<break time="1000" />

<line char="alex">
    ...that we're late.
</line>
```

## 16. Timing Model

Script elements may use the common OVML timing model.

The primary modes are:

afterPrevious
duringCurrent
absolute

### afterPrevious

The element starts after the preceding applicable block has finished.

```xml
<line char="alex">
    Hello!
</line>

<line char="maria" startMode="afterPrevious">
    Hello!
</line>
```

### duringCurrent

The element starts at the specified time relative to the current block.

```xml
<video
    src="rain"
    startMode="duringCurrent"
    startTime="2" />
```

### absolute

The element is positioned at an absolute point on the project timeline.

```xml
<video
    src="intro"
    startMode="absolute"
    startTime="10" />
```

The complete timing model is described in:

concepts/timeline-and-blocks.md
reference/timing.md

## 17. Complete Hierarchy

The conceptual structure of OVML 2.2 is:

```text
ovml
│
├── meta
│   ├── title
│   ├── author
│   └── preferences
│
├── cast
│   └── character
│       ├── voice
│       ├── audio processor
│       ├── video processor
│       └── image processor
│
├── assets
│   └── asset
│
├── world
│   ├── locations
│   │   └── location
│   │       ├── era
│   │       ├── style
│   │       ├── palette
│   │       ├── props
│   │       └── atmosphere
│   │
│   └── terms
│       └── term
│
└── script
    │
    └── chapter
        │
        └── scene
            │
            ├── camera
            │
            ├── p
            │   └── line
            │       └── w
            │
            ├── img
            ├── video
            ├── audio
            │
            └── break
```

This is a conceptual model. Not every element is required in every document.

## 18. Minimal OVML Document

The simplest document can contain only the root element and a script:

```xml
<ovml version="2.2" lang="en">

    <script>

        <chapter title="Greeting">

            <scene>

                <line>
                    Hello, world!
                </line>

            </scene>

        </chapter>

    </script>

</ovml>
```

This is already a valid conceptual OVML project.

## 19. Example Multimedia Document

A more complete project may look like this:

```xml
<ovml version="2.2" lang="en">

    <meta>
        <title>The Last Evening</title>
        <author>OpenVML Example</author>

        <preferences
            showSubtitles="true"
            subtitleFontSize="16" />
    </meta>

    <cast>

        <character
            id="alex"
            name="Alex"
            gender="male"
            age="adult"
            role="protagonist"
            voiceId="en-US-GuyNeural"
            voiceLang="en-US"
            voiceEngine="edge-tts" />

        <character
            id="maria"
            name="Maria"
            gender="female"
            age="adult"
            role="major"
            voiceId="en-US-JennyNeural"
            voiceLang="en-US"
            voiceEngine="edge-tts" />

    </cast>

    <assets>

        <asset
            id="sunset"
            type="image"
            src="https://example.com/sunset.jpg" />

        <asset
            id="music"
            type="audio"
            src="https://example.com/music.mp3" />

    </assets>

    <script>

        <chapter id="chapter-1" title="The Meeting">

            <scene
                color="#1a1a2e"
                atmosphere="warm sunset, calm atmosphere">

                <camera
                    shot="wide"
                    framing="center"
                    movement="static" />

                <img
                    src="sunset"
                    layer="background"
                    sizePercent="100" />

                <audio
                    src="music"
                    volume="0.4"
                    loop="true" />

                <line char="alex">
                    What a beautiful evening.
                </line>

                <line char="maria">
                    Yes. Very quiet.
                </line>

            </scene>

        </chapter>

    </script>

</ovml>
```

## 20. OVML and Project Forms

The same document model can represent different types of content.

For example:

```
Lecture
    chapter
        scene
            camera
            slide/media
            narration

Presentation
    chapter
        scene
            camera
            image
            line

Shorts/Reels
    chapter
        scene
            camera
            video
            line

Audiobook
    chapter
        scene
            camera
            line
            audio

Game Voiceover
    chapter
        scene
            line
            audio
            camera

Film Voiceover
    chapter
        scene
            camera
            video
            line

Anime
    chapter
        scene
            camera
            image/video
            line

Course
    chapter
        scene
            camera
            slide
            line

Podcast
    chapter
        scene
            line
            audio

```
OVML does not require a separate file format for each of these use cases.

The declarative model allows one standard to represent many different production workflows.

## 21. OVML, OVMZ, and OVMV

It is important to distinguish the script from the container/output formats.

### OVML

OVML is the descriptive document itself.

It can exist independently:

project.ovml

and reference external resources:

https://example.com/image.jpg
https://example.com/audio.mp3

An OVML project may therefore remain lightweight and reference resources that are already available
on permitted external locations.

### OVMZ

OVMZ is a project container.

Conceptually:

```text
project.ovmz
│
├── content.ovml
├── project.json
├── resources/
├── presets/
└── tts/
```

It can package the OVML document together with the resources required for a portable project.

### OVMV

OVMV represents a rendered visual result, typically a video:

project.ovmv

In a production workflow, OVMV may be generated by rendering an OVML project into a finished video.

## 22. The Core Separation

The OVML architecture intentionally separates:

```text
description
    │
    ▼
OVML
    │
    ├── content
    ├── characters
    ├── resources
    ├── timing
    ├── scenes
    └── camera
          │
          ▼
Player / Renderer
          │
          ├── buffering
          ├── streaming
          ├── TTS
          ├── decoding
          ├── compositing
          └── rendering
```

Therefore:

OVML defines when content should occur. The Player determines how that content is buffered,
streamed, synthesized, decoded, rendered, and synchronized on the target platform.

This is one of the fundamental architectural principles of OpenVML.

## 23. What OVML Does Not Specify

OVML does not require a particular:

- audio engine;
- TTS engine;
- video codec;
- graphics API;
- browser API;
- operating system;
- buffering strategy;
- streaming mechanism;
- API-key storage mechanism;
- cloud service.

For example, one Player may use:

Howler + WebAudio

another:

native audio APIs

and a renderer:

FFmpeg + GPU

All of them can work with the same OVML document.

## 24. Related Documents

[`spec/OVML-2.2.md`] — normative specification
[`concepts/scenes-and-world.md`] — scene and world canon concepts
[`concepts/camera.md`] — camera model
[`concepts/timeline-and-blocks.md`] — timing model
[`reference/meta.md`] — metadata
[`reference/cast.md`] — characters
[`reference/assets.md`] — assets and resources
[`reference/world.md`] — world canon
[`reference/locations.md`] — canonical locations
[`reference/body.md`] — document body
[`reference/chapter.md`] — chapter model
[`reference/scene.md`] — scene model
[`reference/blocking.md`] — blocking
[`reference/line.md`] — line model
[`reference/paragraph.md`] — paragraph model
[`reference/media.md`] — media elements
[`reference/layers.md`] — layers
[`reference/subtitle.md`] — subtitles
[`reference/timing.md`] — timing semantics
[`reference/break.md`] — pauses