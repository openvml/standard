# OpenVML Architecture

**OpenVML Standard 2.2**

This document explains the overall philosophy and architecture of OpenVML before the individual
elements and attributes are described elsewhere in this section.

OVML is not a video or audio file format. It is a declarative, XML-based description of an
audiovisual work. It describes what a work should contain, how its parts relate to each other, and
when each part should occur — leaving the actual rendering and playback to a compatible application.

## 1. Declarative Intent Over Coordinates

The central idea of OVML is that a document expresses intent, not the final pixels or samples of the
output.

Traditional media formats describe the finished result. An MP4 file describes a rendered video. A
WAV file describes rendered audio. An image file describes a rendered image. If you change any
creative property, you must re-render the entire artifact, and the intermediate data does not tell
you why any particular value was chosen.

OVML takes the opposite approach. It describes the structure and intentions of the work:

    who speaks;
    what they say;
    which voice should be used;
    when a line starts;
    which asset is displayed;
    where the asset is positioned;
    how large it is;
    how long it appears;
    which scene is active;
    the atmosphere of a scene;
    how the camera should behave;
    how subtitles are presented.

None of these statements are coordinates. They are declarative instructions. The Player or renderer
is free to realize them using any technology it chooses.

For example:

```xml
    <video
        src="background"
        layer="background"
        duration="10" />
```

does not require a specific video library. One Player may use HTML5 video, another FFmpeg, another a
platform media framework. All of them can honor the same instruction.

This is the reason OVML is well suited to content that is continuously edited, regenerated,
translated, re-voiced, or rendered into different target formats: the creative work lives in the
description, not in a one-time binary output.

## 2. Document Versus Runtime

OVML strictly separates the document from the runtime.

An OVML document is a portable description of a work. It can be:

    edited;
    validated;
    translated;
    re-voiced;
    packaged;
    rendered.

A Player or runtime is the application that interprets that document on a target platform. The
runtime decides:

    how content is buffered;
    how it is streamed;
    how TTS is synthesized or resolved;
    how media is decoded;
    how scenes are composited;
    how the camera is realized;
    how playback is synchronized.

The same OVML document may therefore be played by different Players with completely different
internal implementations. The document remains the portable, stable description of the work, while
the implementation brings it to life.

The OpenVML ecosystem expresses this separation through distinct roles:

    the Standard defines the language;
    the Studio creates content;
    the Player executes it.

## 3. The Three-Level Model

OpenVML distinguishes three concerns about a document. Keeping them separate is fundamental to how
the standard works.

Structural validity — whether the document follows the OVML XML structure and uses allowed values.
This is the concern of a validator. It is purely about form: elements properly opened and closed,
required attributes present, enumerations containing allowed values, references resolving to
existing entities.

Semantic interpretation — what a conforming Player should understand from the document. The standard
describes the meaning of the elements and attributes. For example, a validator determines whether an
emotion value is an allowed enumeration value; the standard explains what an emotion means. It does
not judge whether a creative decision is correct or appropriate.

Runtime behavior — how a particular Player implements playback on a platform. Validation and
interpretation do not predict runtime behavior. Whether a TTS provider is available, whether a codec
is supported, whether a remote server can be reached — all of these are runtime concerns.

A validator is not an artistic critic. Overlapping speech, video, audio, and images are completely
valid OVML behavior when their timing relationships are correctly specified. Whether a sad scene
"should" be blue, or whether an overlap is good design, is outside the responsibility of the parser.

## 4. Content, Voice, and Timing Separation

OVML keeps three dimensions of a work independent of each other.

    Content — what is said and shown. Dialogue, narration, images, video, audio.

Voice — who says it and how the speech is intended to sound. A character references a voice
resource; a line may override that character's delivery for a particular moment.

    Timing — when each element occurs and how elements relate to each other on the timeline.

Because content, voice, and timing are separate, they can change independently. Re-voicing a project
does not require rewriting dialogue. Retiming a scene does not require changing the text. Changing a
voice does not create a new character.

This independence is what makes OVML suitable for AI-assisted production: a structured document can
be modified in one dimension without regenerating the entire work as an opaque binary.

## 5. Language-Agnostic

OVML is deliberately language-agnostic.

The document declares its primary language in the root element:

```xml
    <ovml version="2.2" lang="en">
```

The textual content of a project — dialogue, narration, subtitles, scene atmosphere — is written in
the language of the work. OVML does not hardcode a particular human language, and no vocabulary of
the language presumes one.

Identifiers are also language-agnostic in a specific sense. Character identifiers and other stable
ids are held in the original script, not transliterated or translated freely. This lets the same
document model serve a Ukrainian audiobook, an English lecture, or a Russian presentation without
changing the format.

## 6. The Projection Rule

Because OVML describes intent rather than final output, a single document can project onto many
different renderers.

The same OVML project can be:

    played by a TTS-centric Player that produces an audiobook audio stream;
composed into a video by a renderer that interprets scenes, layers, camera, and processing presets;
    read start to finish by a text reader that presents the script;
    analyzed by an AI system that extracts characters, scenes, and relationships.

Each application consumes the same source and realizes it according to its own capabilities. A
renderer is not required to support every feature equally; it preserves the semantics it supports
and clearly defines what it cannot do.

The projection rule does not mean every renderer produces identical output. It means the source is
portable and the mapping from source to output belongs to the implementation, not to the formatter.
This is why the standard does not require a single rendering engine, audio library, video library,
UI framework, or TTS provider. The OVML document remains the common portable description of the
work.

## 7. Summary

The architecture of OpenVML can be summarized as follows.

    Declarative intent — OVML describes the desired experience, not the output coordinates.

    Document versus runtime — the portable description is separate from the Player that executes it.

Three-level model — structural validity, semantic interpretation, and runtime behavior are distinct.

    Content, voice, timing — three independent dimensions of the work.

    Language-agnostic — vocabulary is defined once and works for any human language.

Projection — one document can map onto TTS, video composition, reading, and AI-assisted workflows.

The guiding principle is captured in a single line from the reference:

OVML defines what should happen and when. The Player determines how that content is buffered,
streamed, synthesized, decoded, rendered, and synchronized on the target platform.

See: reference/document.md
See: reference/validation.md
See: reference/README.md
