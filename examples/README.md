# OpenVML Audiobook Example

This example demonstrates how **OpenVML — Open Voice Markup Language** can represent a narrated audiobook with multiple characters, dialogue, voice assignments, timed media, and audio processing.

It is the first application example in the OpenVML standard documentation.

## Purpose

The purpose of this example is to demonstrate how a narrative audiobook can be represented as an OpenVML document while combining spoken content with additional media.

The example contains:

* a narrator;
* multiple named characters;
* character aliases;
* character personality and backstory;
* voice assignments;
* voice language and provider information;
* audio processing presets;
* narration;
* character dialogue;
* multiple speakers within a paragraph;
* background video;
* foreground audio;
* foreground video;
* foreground images;
* media duration;
* relative media timing;
* grid-based positioning;
* media scaling;
* scenes;
* scene metadata;
* chapters.

## Characters

The document defines a cast containing a narrator and six characters.

Each character may contain information such as:

* `id`;
* `name`;
* `color`;
* `gender`;
* `age`;
* `role`;
* `voiceId`;
* `voiceEngine`;
* `voiceName`;
* `voiceLang`;
* `pitch`;
* `rate`;
* `aliases`;
* `personality`;
* `backstory`;
* audio processing information.

For example, a character can have several aliases:

`` `<character
    id="mira"
    name="Mira"
    aliases="little singer|college student|blonde girl|fair-haired girl|young performer"
    ...
/>
`` `
The aliases identify different textual descriptions that refer to the same character.

When the character speaks, the canonical character identifier is used:

`` `<line char="mira">...</line>
`` `
A descriptive reference to the same character may instead occur as part of narration:

`` `<line char="narrator">
    — squeaked the little singer...
</line>
`` `
The description does not replace the canonical character identifier.

## Narration and Dialogue

OpenVML allows narration and dialogue to coexist within the same paragraph.

For example:

`` `<p>
    <line char="mira">— If we don't start the show...</line>
    <line char="narrator">— squeaked the little singer...</line>
</p>
`` `
A paragraph may contain several alternating speakers:

`` `<p>
    <line char="bran">— Stop whining.</line>
    <line char="narrator">— growled the tattooed bruiser.</line>
    <line char="bran">— Hey, bartender, pour me another one.</line>
</p>
`` `
This makes it possible to preserve the order of speech and narration without flattening the content into a single voice.

## Chapters and Scenes

The document is organized into chapters.

The example contains:

* a character reference section;
* an opening scene;
* a dialogue scene.

The dialogue chapter also contains scene metadata:

`` `<scene title="Tensions in the Rusty Anchor" mood="Tense">
    <location>The Rusty Anchor Tavern</location>
</scene>
`` `
This demonstrates that a scene may contain semantic information in addition to its spoken content.

## Media

The audiobook example demonstrates that spoken content may coexist with other media.

The document uses:

* background video;
* foreground audio;
* foreground video;
* foreground image.

For example:

`` `<video
    src="..."
    layer="background"
    duration="100"
    sizePercent="100"
/>
`` `
A foreground video can have its own duration and start relationship:

`` `<video
    src="..."
    layer="foreground"
    duration="10"
    startTime="0"
    startMode="duringCurrent"
    gridRow="2"
    gridCol="2"
    gridRowSpan="2"
    gridColSpan="2"
    sizePercent="30"
/>
`` `
The foreground media therefore does not need to begin or end at the same time as the surrounding spoken content.

## Timing

OpenVML allows media and spoken content to coexist on a shared timeline.

A background element may continue while narration and dialogue occur above it.

Additional foreground media may appear during an active content block and may have its own:

* start time;
* start mode;
* duration;
* layer;
* position;
* size.

This allows an audiobook to use a multimedia presentation model without requiring the spoken content to become a conventional linear audio track.

The Player is responsible for implementing buffering, streaming, synthesis, decoding, rendering, and synchronization for the target platform.

## Assets

Reusable media can be declared in the document's asset collection:

`` `<assets>
    <asset id="..." type="image" src="..."/>
    <asset id="..." type="video" src="..."/>
</assets>
`` `
Content can then reference the corresponding asset identifier.

This separates resource identification from the content blocks that use those resources.

## Voice Processing

Characters may reference audio processing presets:

`` `audioProcessorId="..."
audioProcessorName="..."
`` `
The processing itself is defined separately in the OpenVML preset collection.

Audio processing may include effects such as:

* EQ;
* compressor;
* limiter;
* normalizer;
* noise reduction;
* reverb;
* delay;
* chorus;
* gain;
* pan;
* fade;
* trim;
* de-esser;
* de-clicker;
* output conversion.

The audiobook example uses voice processing presets to demonstrate this relationship.

## Visual Presentation

Although the example is audiobook-oriented, OpenVML does not require spoken content to be audio-only.

The same document model can associate speech with visual media.

OpenVML can also represent presentation properties such as:

* font;
* font size;
* font weight;
* text color;
* background;
* alignment;
* text shadow;
* capitalization;
* word-by-word display;
* scrolling text;
* grid position;
* transitions;
* keyframes.

These capabilities are part of the language and are not limited to audiobook projects.

## What This Example Demonstrates

The example demonstrates an important OpenVML principle:

> **OVML defines when content should occur. The Player determines how that content is buffered, streamed, synthesized, decoded, rendered, and synchronized on the target platform.**

The document therefore describes content and temporal relationships without prescribing a particular playback implementation.

## Relation to the Standard

This file is an informative example.

It does not redefine the OpenVML language.

The normative definitions of individual elements, attributes, timing rules, media behavior, cast properties, and processing presets are specified in the corresponding documents under:

```docs/standard/concepts/
docs/standard/reference/
docs/standard/presets/
`` `
The example should be read together with those reference documents.

## Future Examples

This audiobook example is the first application example.

Future OpenVML examples may cover other domains, including:

* lectures;
* presentations;
* Shorts and Reels;
* courses;
* podcasts;
* game voiceover;
* film dubbing;
* anime;
* interactive multimedia.

The list is intentionally open-ended.

OpenVML is designed as a general voice and multimedia timing language rather than as a format restricted to audiobooks.
