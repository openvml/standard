# OVML Concepts

**OpenVML — Open Voice Markup Language** is an open, XML-based format for describing structured audiovisual content.

This section explains the concepts behind OVML 2.2 before going into individual XML elements and attributes.

OVML is designed to describe **what should happen, when it should happen, and how different pieces of content relate to each other**. It does not prescribe how a particular Player must implement playback.

> **OVML defines when content should occur. The Player determines how that content is buffered, streamed, synthesized, decoded, rendered, and synchronized on the target platform.**

This separation allows the same OVML document to be used across different players, platforms, rendering engines, and content-production tools.

## What OVML Is For

OVML can describe a wide range of audiovisual projects, including:

* **Lecture / Lesson** — structured educational content with chapters, narration, slides, subtitles, and synchronized media.
* **Presentation** — slide-based content with synchronized narration, animations, and speaker notes.
* **Shorts / Reels / TikTok** — short-form vertical video with narration, subtitles, background media, and timed foreground elements.
* **Audiobook** — narrated books with multiple characters, voices, sound effects, music, chapters, and synchronized text.
* **Game Voiceover** — character dialogue, voice direction, timed events, and interactive narrative content.
* **Film Voiceover / Dubbing** — multi-character dialogue, timed media, voice direction, and audiovisual synchronization.
* **Anime** — animated scenes with character dialogue, background media, music, effects, and scene direction.
* **Course** — structured educational programs combining narration, slides, text, media, and interactive material.
* **Podcast** — multi-speaker audio content with narration, music, sound effects, introductions, transitions, and timed elements.
* **Interactive Multimedia Projects** — audiovisual compositions in which several media elements may overlap and start independently.

## OVML as a Composition Language

OVML is not limited to a linear sequence of media files.

A project may contain:

* background video or audio;
* foreground video, audio, or images;
* spoken dialogue;
* subtitles and text;
* multiple simultaneous media elements;
* scenes;
* camera instructions;
* pauses;
* timed transitions;
* word-by-word text synchronization;
* visual overlays;
* media positioned in different grid cells.

For example, a background video may continue playing while a TTS voice speaks over it. A foreground image, video, or sound effect may then begin **in the middle of that speech**, without requiring the background media or the speech block to end.

This makes OVML a **temporal composition language**, rather than simply a playlist format.

## Timing and Synchronization

OVML separates the existence of content from its temporal relationship with other content.

A content block may:

* begin after a previous block;
* begin at an absolute timeline position;
* begin at a specified time relative to another active block;
* overlap another block;
* continue while other blocks start or finish;
* have an explicit duration;
* be looped;
* be positioned independently within the visual composition.

The timing model is described in [`reference/timing.md`](../reference/timing.md) and [`timeline-and-blocks.md`](timeline-and-blocks.md).

## Scenes

A `<scene>` groups content that belongs to a particular narrative or visual context.

A scene may provide:

* a visual color identifier;
* an atmospheric description;
* a boundary for a sequence of content blocks;
* context that can be used by production tools.

The `<scene>` element is described in [`reference/scene.md`](../reference/scene.md). The scene concept is in [`scenes-and-world.md`](scenes-and-world.md).

## Camera

The `<camera>` element describes camera or viewport behavior within a scene.

Camera instructions are part of the content description rather than an implementation-specific command to a particular rendering engine.

The camera concept is in [`camera.md`](camera.md). The camera element reference is in [`reference/scene.md`](../reference/scene.md).

## Locations and the World Canon

Long-form content contains recurring entities. A novel has many scenes, and the same location recurs across them; a course repeats its terms; a documentation set repeats its definitions.

If each scene repeats its own description of an entity, those descriptions drift apart: a tavern described as "dark" in one scene and "bright" in the next, a prop that appears in one scene but is forgotten in the next, a term defined one way in the intro and another way in a later lesson.

OVML solves this with a **world canon**.

The document declares its canonical entities once, in a `<world>` container at the top of the document:

```xml
<world>
    <locations>
        <location id="rusty_anchor" name="The Rusty Anchor" type="tavern">
            <era>fantasy-medieval</era>
            <style>rough-hewn oak, candlelight</style>
            <palette>hearth:#4a2f1a; walls:#3a2a1e</palette>
            <props>cracked bar, long benches, hearth</props>
            <atmosphere>low murmur, smell of ale and smoke</atmosphere>
        </location>
    </locations>
</world>
```

A scene then references the canon by id instead of duplicating it:

```xml
<scene>
    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>
    ...
</scene>
```

`<world>` does not prescribe a fixed set of sections. A document declares the sections that its content requires, and every section follows the same uniform rule — so a lecture's glossary uses exactly the same mechanism as a novel's locations.

The canon holds permanent properties. Scene-specific change is expressed as a `<variation>` in the scene, never by editing the canon.

The standard does not define project types. A parser interprets a document from its structure: whichever sections are present are parsed by the uniform rule.

This gives the author, the Player, and the AI Assistant a single stable description of the project's recurring entities — a form of memory that survives across scenes, chapters, and `.ovml`/`.ovmz` transfers.

The world canon is described in [`reference/world.md`](../reference/world.md) and [`reference/locations.md`](../reference/locations.md). The concept is in [`scenes-and-world.md`](scenes-and-world.md) and [`extensibility.md`](extensibility.md).

## Characters and Voices

OVML separates a character from the voice used to represent that character.

A character may contain:

* identity;
* name;
* aliases;
* gender;
* age;
* narrative role;
* character color;
* personality;
* backstory;
* voice information;
* optional processing presets.

The Player or another compatible runtime may use this information to resolve and render speech.

The character model is described in [`reference/cast.md`](../reference/cast.md) and [`reference/character.md`](../reference/character.md). The concept is in [`characters-and-voices.md`](characters-and-voices.md).

## Media Composition

OVML supports multiple media types as independent content elements.

Media may include:

* video;
* audio;
* images;
* text;
* synthesized speech;
* external resources.

Media elements can participate in the same timeline and may overlap one another.

See [`reference/media.md`](../reference/media.md), [`reference/assets.md`](../reference/assets.md), and [`media-layers.md`](media-layers.md).

## Assets

OVML does not require every media resource to be physically embedded in the document.

An asset may be:

* included in an OVMZ bundle;
* referenced by a path within a bundle;
* referenced by an external URL;
* referenced through another permitted resource mechanism supported by the Player.

This allows the same OVML content model to be used both as a self-contained project and as a lightweight script that references external resources.

## Standalone OVML

An OVML document does not have to be packaged as an OVMZ project.

A simple `.ovml` document may reference externally available media and other permitted resources.

This makes OVML suitable for:

* streaming scenarios;
* web-based playback;
* lightweight scripts;
* content sharing;
* remote resources;
* projects that do not require bundling.

## OVMZ and OVMV

OVML describes the content.

It may be transported or packaged in different forms.

### OVML

A standalone XML document containing the content description.

```text
project.ovml
```

### OVMZ

A packaged project containing an OVML document together with resources, presets, metadata, and optionally pre-rendered TTS assets.

```text
project.ovmz/
├── content.ovml
├── project.json
├── resources/
├── presets/
└── tts/
```

### OVMV

OVMV is a distribution/rendered form of an OVML project intended for playback as a packaged audiovisual experience.

The distinction between the content description and its distribution or rendering form is intentional.

## Standard, Studio, and Player

OpenVML projects may pass through several components, but these components have different responsibilities.

### OpenVML Standard

Defines:

* document structure;
* XML elements;
* attributes;
* semantics;
* timing relationships;
* media composition;
* preset document formats;
* packaging conventions.

### OpenVML Studio

Creates and edits OVML projects.

Studio may provide:

* project authoring;
* character management;
* voice selection;
* TTS generation;
* asset selection;
* preset creation;
* previews;
* project export.

These are Studio capabilities and are not automatically part of the OVML Standard.

### OpenVML Player

Consumes OVML content and determines how it is executed on a target platform.

The Player may:

* resolve assets;
* obtain or synthesize speech;
* decode media;
* apply processing;
* buffer and stream content;
* synchronize multiple media elements;
* render scenes and camera instructions.

Different Players may implement these operations differently while consuming the same OVML document.

## Design Principle

The central architectural principle of OVML is:

> **The Standard defines the language. The Studio creates content. The Player executes it.**

This separation allows OVML to remain independent of any single authoring application, rendering engine, TTS provider, operating system, or hardware platform.

## Further Reading

* [`packaging.md`](packaging.md) — forms in which an OVML project can exist.
* [`document-model.md`](document-model.md) — the document tree and container model.
* [`semantic-model.md`](semantic-model.md) — intent vs. implementation, semantic layering.
* [`../reference/document.md`](../reference/document.md) — complete document hierarchy.
* [`../reference/timing.md`](../reference/timing.md) — timing and overlapping content.
* [`../reference/scene.md`](../reference/scene.md) — scene element reference.
* [`camera.md`](camera.md) — camera concept.
* [`../reference/cast.md`](../reference/cast.md) — characters and voices.
* [`../reference/character.md`](../reference/character.md) — character element reference.
* [`../reference/world.md`](../reference/world.md) — world canon and entity references.
* [`../reference/locations.md`](../reference/locations.md) — locations canon and scene refs.
* [`../reference/media.md`](../reference/media.md) — audiovisual blocks.
* [`../reference/assets.md`](../reference/assets.md) — asset references.
* [`cross-references.md`](cross-references.md) — the ID/reference system across the document.
* [`architecture.md`](architecture.md) — high-level architectural principles.