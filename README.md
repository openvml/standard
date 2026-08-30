# OpenVML Standard

## OVML — Open Voice Markup Language

**OVML** is an open, declarative XML-based standard for describing audiovisual content, interactive narratives,
synchronized speech, media assets, scenes, timing, characters, and presentation.

OVML is designed to describe **what an audiovisual work should contain and how its elements are structured**, while
leaving actual rendering and playback to compatible applications.

The standard is designed for:

- multimedia documents;
- educational content;
- presentations;
- audiobooks;
- podcasts;
- short-form video;
- game dialogue;
- film and anime voiceover;
- interactive narratives;
- AI-assisted media production.

The official OpenVML ecosystem includes:

- **OpenVML Standard** — the format specification;
- **OpenVML Player** — an open-source runtime for playing OVML projects;
- **OpenVML Studio** — an authoring environment for creating and assembling OVML projects;
- **OpenVML Cloud / Publishing** — optional online services for publishing and distributing OpenVML projects.

---

## Why OVML?

Traditional media formats describe the **final result**.

For example:

- MP4 describes a rendered video;
- WAV/MP3 describes rendered audio;
- an image file describes a rendered image.

OVML describes the **structure and intentions of the work**.

A single OVML document can describe:

- who speaks;
- what they say;
- which voice should be used;
- when a line starts;
- which media asset is displayed;
- where the asset is positioned;
- how long it is displayed;
- which scene is active;
- the atmosphere of a scene;
- how the camera should behave;
- how subtitles are displayed;
- how chapters and blocks are organized.

This makes OVML particularly suitable for projects where content is continuously edited, regenerated, translated,
re-voiced, or rendered into different target formats.

---

# Core principle

The central architectural idea of OpenVML is:

> **OVML describes the experience. The Player renders the experience.**

The standard therefore does not define a particular rendering engine, audio library, video library, UI framework,
operating system, or TTS implementation.

A compatible implementation is free to choose its own:

- audio backend;
- video backend;
- TTS provider;
- graphics engine;
- UI framework;
- storage system;
- operating system integration;
- rendering pipeline.

The OVML document remains the portable description of the work.

---

# What can be created with OVML?

OVML is intentionally not limited to one type of media.

## Lecture / Lesson

**Lecture**

Educational content with structured chapters, synchronized narration, slides, media assets, timestamps, and a clear
narrative.

Suitable for:

- e-learning;
- tutorials;
- university lectures;
- training materials;
- educational videos.

---

## Presentation

**Presentation**

Slide-oriented content with synchronized narration, animations, media assets, and speaker notes.

Suitable for:

- business presentations;
- product demonstrations;
- education;
- conference presentations;
- narrated slide decks.

---

## Shorts / Reels / TikTok

**Shorts / Reels**

Short-form vertical content with synchronized narration, subtitles, background media, scenes, and timing.

Suitable for:

- YouTube Shorts;
- Instagram Reels;
- TikTok;
- promotional clips;
- social-media storytelling.

---

## Audiobook

**Audiobook**

Long-form narrated books with chapters, multiple characters, voices, dialogue, sound effects, music, and navigation.

OVML can describe:

- narrator voice;
- character voices;
- dialogue;
- scene structure;
- background audio;
- chapter navigation;
- synchronized text.

This makes OVML suitable for multi-voice and AI-assisted audiobooks.

---

## Game Voiceover

**Game Voiceover**

Character dialogue and interactive narration with individual voices and structured dialogue blocks.

Potential applications include:

- game dialogue;
- NPC voices;
- interactive stories;
- branching narratives;
- character voice libraries.

---

## Film Voiceover

**Film Voiceover**

Multi-character voiceover and dubbing projects with synchronized dialogue, scenes, timing, audio processing, and media
assets.

OVML can be used as a structured intermediate representation before final audio/video rendering.

---

## Anime

**Anime**

Animated stories with scenes, characters, dialogue, emotions, music, sound effects, and visual direction.

OVML provides a structured layer between the screenplay and the final rendered media.

---

## Course

**Course**

Structured educational programs containing chapters, lessons, narration, slides, media, and interactive elements.

---

## Podcast

**Podcast**

Conversational audio projects with multiple speakers, introductions, transitions, music, sound effects, and structured
episodes.

---

# OVML, OVMZ and OVMV

OpenVML uses different representations for different stages of the media lifecycle.

## OVML

**OVML is the source description.**

It contains the structured description of the work:

``text`
characters
scenes
chapters
dialogue
media
timing
camera
presentation
```

OVML is editable and intended to remain human- and machine-readable.

OVMZ

OVMZ is a project container.

An OVMZ package can contain:

project
├── content.ovml
├── project metadata
├── media resources
├── presets
└── pre-rendered or cached resources

OVMZ is intended to make a project portable.

A project can therefore be distributed as a single container while retaining the original OVML structure.

OVMV

OVMV is a rendered video container/output.

When a project is rendered into a final video, the OVML description is converted into a conventional audiovisual result.

Conceptually:

OVML
  │
  │ render
  ▼
OVMV

Whereas:

OVML
  │
  │ package
  ▼
OVMZ

preserves the structured project.

OVML document structure

A basic OVML document has the following structure:

<ovml version="2.2" lang="en">

``xml`
   <meta>
       ...
   </meta>

   <cast>
       ...
   </cast>

   <assets>
       ...
   </assets>

   <script>
       ...
   </script>
```
</ovml>

The major sections are:

Section	Purpose
<ovml>	Root document
<meta>	Project metadata and presentation preferences
<cast>	Characters and voice configuration
<assets>	Media resources
<script>	Chapters, scenes, and content blocks

Additional structural elements are defined by the specification.

Scenes

OVML supports explicit scene descriptions using the <scene> element.

A scene can contain visual and narrative information such as:

<scene
``xml`
   color="#1a1a2e"
   atmosphere="warm sunset, silence, tranquility">

   ...

</scene>
```

The color attribute can be used as a visual hint for UI presentation and processing presets.

The atmosphere attribute provides a semantic description of the scene.

This information can be used by authoring tools and AI-assisted workflows to help select:

background media;
music;
sound effects;
image assets;
visual processing presets.

A scene therefore provides a semantic layer between the script and the final media composition.

Camera

Starting with OVML 2.2, the standard also introduces the <camera> element.

<camera> describes visual direction independently from the physical rendering implementation.

The purpose is to allow an OVML document to express cinematic intent such as:

camera position;
framing;
shot type;
movement;
transition;
focus;
duration;
visual emphasis.

The exact <camera> vocabulary is defined in the OVML 2.2 specification.

The important architectural principle is:

OVML describes the camera intent; the renderer decides how to realize it.

This allows different renderers to implement the same OVML project using different graphics and video technologies.

Characters and voices

Characters are declared in the <cast> section.

Example:

``xml`
<cast>

   <character
       id="alex"
       name="Alex"
       gender="male"
       age="adult"
       role="protagonist"
       voiceId="ru-RU-DmitryNeural"
       voiceName="Dmitry"
       voiceLang="ru-RU"
       voiceEngine="edge-tts"
       pitch="1"
       rate="1" />

</cast>
```

A character can contain information about:

identity;
display name;
gender;
age;
narrative role;
voice;
language;
pitch;
speech rate;
audio processing;
video processing;
image processing.

The standard does not require one specific TTS engine.

A compatible implementation may use:

local TTS;
cloud TTS;
operating-system TTS;
AI voice providers;
user-provided voice engines.
Script

The <script> section contains the actual content of the work.

Content is organized into structural units such as:

chapters;
scenes;
text/dialogue blocks;
media blocks;
pauses;
synchronized word groups.

Example:

``xml`
<line

   char="alex"
   startMode="afterPrevious"
   startDelay="0"
   wordByWord="true"
   wordByWordMode="single"
   wordDisplayDuration="100">

   Hello, world!

</line>
```
Timing

Timing is an important part of OVML.

Content can be synchronized using different start modes.

afterPrevious

The block starts after the previous textual block has finished.

duringCurrent

The block starts at a specified time relative to the current timing context.

absolute

The block starts at an absolute position on the timeline.

Example:

``xml`
<video

   src="background-video"
   startMode="absolute"
   startTime="10"
   duration="20" />
```
This allows an OVML document to describe complex synchronization without embedding a particular playback engine into the
format.

Media

OVML can reference different types of media:

audio;
video;
images;
speech/TTS;
other resources supported by a compatible implementation.

Example:

``xml`
<video

   src="asset_id"
   layer="background"
   volume="1"
   duration="10"
   startTime="0"
   startMode="absolute" />
```
The src value identifies the resource.

The actual resource may be stored:

externally;
locally;
inside an OVMZ container;
in a project library;
in a compatible cloud storage system.
Word-by-word presentation

OVML supports synchronized word presentation.

Example:

``xml`
<line

   wordByWord="true"
   wordByWordMode="cumulative"
   wordDisplayDuration="500">

   <w group="1">Hello</w>
   <w group="1">world</w>
   <w group="2">how</w>
   <w group="2">are</w>
   <w group="2">you?</w>

</line>
```

This can be used for:

karaoke-style presentation;
language learning;
subtitles;
reading assistance;
synchronized narration;
accessibility.
Declarative format

OVML is a declarative format.

An OVML document describes the desired structure and timing of an audiovisual experience.

It does not prescribe how a particular application must implement it.

For example, the following:
``xml`
<video src="background" duration="10" />
```
does not require a specific video library.

A Player may use:

HTML5 video;
native video APIs;
FFmpeg;
platform media frameworks;
WebAssembly;
another compatible implementation.

The same principle applies to audio and TTS.

OpenVML architecture

The OpenVML ecosystem separates description from execution.
``text`
                 OVML Standard
                      │
                      │ describes
                      ▼
              OpenVML Project
                      │
          ┌───────────┴───────────┐
          │                       │
       OVMZ                    OVMV
   project package          rendered video
          │
          ▼
   OpenVML Player
          │
          ▼
       Runtime
```
The standard defines the portable representation.

The Player is responsible for execution.

The Studio is responsible for authoring and project preparation.

Cloud services are optional infrastructure around the format.

AI-assisted authoring

OVML is particularly suitable for AI-assisted media production.

An AI assistant can work with the structured document rather than directly manipulating rendered media.

For example:
``text`
idea
  ↓
story
  ↓
chapters
  ↓
scenes
  ↓
characters
  ↓
dialogue
  ↓
assets
  ↓
timing
  ↓
OVML
  ↓
OVMZ / OVMV
```
Because the content is structured, an AI system can modify individual elements without having to regenerate the entire
project.

For example, changing:

a character's voice;
a dialogue line;
a scene atmosphere;
an image;
a video asset;
timing;
subtitles;
camera direction

does not require treating the entire work as an opaque binary file.

Compatibility

A conforming OVML implementation should:

correctly parse the document structure;
validate required elements and attributes;
preserve unknown information where appropriate;
correctly interpret supported timing semantics;
resolve referenced characters and resources;
respect the semantics defined by the specification.

The parser/validator is responsible for determining whether an OVML document is structurally valid.

Creative or directorial decisions are not validation errors.

For example, the standard can determine that:
``xml`
<scene>
    ...
</scene>

is correctly formed and properly closed.

Whether the scene itself is artistically appropriate is outside the responsibility of the parser.

Versioning

The current standard is:

OVML 2.2

The version is declared in the root element:
``xml`
<ovml version="2.2" lang="en">
```
Implementations should use the declared version to determine which vocabulary and semantics are supported.

Future versions may introduce new:

elements;
attributes;
media types;
timing mechanisms;
camera controls;
processing hints;
extension mechanisms.

Backward compatibility will be considered when evolving the standard.

Specification

The normative specification is located in:

spec/

The main specification is:

spec/OVML-2.2.md

Additional documents describe individual parts of the format.

Recommended reading order for the reference (normative element/attribute definitions):

1. [`reference/document.md`](reference/document.md) — root `<ovml>`, hierarchy, sections
2. [`reference/meta.md`](reference/meta.md) — metadata and presentation preferences
3. [`reference/cast.md`](reference/cast.md) — `<cast>` container
4. [`reference/character.md`](reference/character.md) — `<character>` element
5. [`reference/body.md`](reference/body.md) — `<script>` content model
6. [`reference/chapter.md`](reference/chapter.md) — `<chapter>` element
7. [`reference/scene.md`](reference/scene.md) — `<scene>` and `<camera>` elements
8. [`reference/line.md`](reference/line.md) — `<line>` element
9. [`reference/paragraph.md`](reference/paragraph.md) — `<p>` element
10. [`reference/media.md`](reference/media.md) — `<video>`, `<audio>`, `<img>`
11. [`reference/subtitle.md`](reference/subtitle.md) — `<subtitle>` element
12. [`reference/assets.md`](reference/assets.md) — asset passports and `assetRef`
13. [`reference/world.md`](reference/world.md) — `<world>` container
14. [`reference/locations.md`](reference/locations.md) — `<locations>` and `<location>` entities
15. [`reference/break.md`](reference/break.md) — `<break>` element
16. [`reference/timing.md`](reference/timing.md) — temporal model
17. [`reference/layers.md`](reference/layers.md) — layer system
18. [`reference/blocking.md`](reference/blocking.md) — semantic character relationships
19. [`reference/emotions.md`](reference/emotions.md) — emotion/intonation vocabularies
20. [`reference/transitions.md`](reference/transitions.md) — transition types
21. [`reference/audio-processing.md`](reference/audio-processing.md) — `<audio_processing>`
22. [`reference/video-processing.md`](reference/video-processing.md) — `<video_processing>`
23. [`reference/image-processing.md`](reference/image-processing.md) — `<image_processing>`
24. [`reference/tts.md`](reference/tts.md) — TTS engine/voice resolution
25. [`reference/voice.md`](reference/voice.md) — voice attributes and SSML controls
26. [`reference/identifiers.md`](reference/identifiers.md) — ID/reference system
27. [`reference/enums.md`](reference/enums.md) — consolidated enum vocabularies
28. [`reference/validation.md`](reference/validation.md) — conformance and XSD

Conceptual background is in the [`concepts/`](concepts/) section.
Examples

Example OVML projects are available in:

examples/

They demonstrate different types of content, including:

minimal projects;
lectures;
presentations;
audiobooks;
short-form video;
voiceover projects.

Examples are intended to be both documentation and compatibility tests for implementations.

Implementations

OpenVML is designed to support multiple independent implementations.

The official OpenVML ecosystem includes:

OpenVML Player

Open-source cross-platform player and runtime.

Repository:

https://github.com/openvml/player

OpenVML Plugins

Public plugin ecosystem for extending OpenVML Player.

Repository:

https://github.com/openvml/plugins

OpenVML Standard

This repository contains the open OVML specification.

Repository:

https://github.com/openvml/standard

Implementations are not required to use OpenVML Player.

Any compatible application can implement the OVML standard independently.

Contributing

The OVML standard is intended to evolve through discussion and implementation experience.

Contributions are welcome in areas including:

specification improvements;
clarification of semantics;
examples;
interoperability;
validation rules;
documentation;
implementation feedback;
new use cases.

When proposing a change to the standard, explain:

the problem;
the proposed solution;
compatibility implications;
example OVML;
impact on existing implementations.

Changes to the normative specification should be made deliberately because the standard is a public contract between
independent implementations.

License

The OpenVML Standard specification is released under the Apache License 2.0.

Apache License 2.0 permits reuse, modification, distribution, and implementation of the specification, including in
commercial software. The license also includes an express patent license.

See:

LICENSE

for the complete license text.

OpenVML

Open format.
Open runtime.
Open ecosystem.

OVML is intended to become a common, portable description layer for audiovisual experiences — from a simple narrated
lesson to a multi-character audiobook, interactive story, animated production, or fully rendered video.

The format describes the work.

The implementation brings it to life.


---

## Repository Structure

``text`
docs/standard/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── spec/
│   └── OVML-2.2.md
├── concepts/
│   ├── architecture.md
│   ├── audio-processing.md
│   ├── camera.md
│   ├── characters-and-voices.md
│   ├── cross-references.md
│   ├── document-model.md
│   ├── extensibility.md
│   ├── media-layers.md
│   ├── packaging.md
│   ├── README.md
│   ├── scenes-and-world.md
│   ├── semantic-model.md
│   ├── timeline-and-blocks.md
│   └── visual-processing.md
├── presets/
│   ├── audio.md
│   ├── video.md
│   ├── image.md
│   └── README.md
├── reference/
│   ├── assets.md
│   ├── audio-processing.md
│   ├── blocking.md
│   ├── body.md
│   ├── break.md
│   ├── cast.md
│   ├── character.md
│   ├── chapter.md
│   ├── document.md
│   ├── emotions.md
│   ├── enums.md
│   ├── identifiers.md
│   ├── image-processing.md
│   ├── layers.md
│   ├── line.md
│   ├── locations.md
│   ├── media.md
│   ├── meta.md
│   ├── paragraph.md
│   ├── scene.md
│   ├── subtitle.md
│   ├── timing.md
│   ├── transitions.md
│   ├── tts.md
│   ├── validation.md
│   ├── video-processing.md
│   ├── voice.md
│   ├── world.md
│   └── README.md
├── schema/
│   └── ovml-2.2.xsd
└── examples/
``xml`
   ├── audiobook/
   │   ├── README.md
   │   ├── source.txt
   │   └── example.ovml
   ├── ner_test.ovml
   ├── README.md
   └── source/
       └── ner_test.md
```

The `spec/` folder holds the normative specification (`OVML-2.2.md`). The `reference/` folder
contains the normative element/attribute reference for each element. The `concepts/` folder
explains the design principles and mental models. The `presets/` folder documents processing
presets. The `schema/` folder holds the XSD. The `examples/` folder contains example OVML projects.