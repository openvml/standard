# Visual Processing

**OpenVML Standard 2.2**

This document explains the conceptual model of visual (video and image) processing in OVML: color
grading, crops, aspect, background removal, and brand-preset workflows — all expressed declaratively
and non-destructively, then interpreted by the runtime.

Visual processing is how an OVML project describes the intended look of its imagery and its brand,
without committing to a particular graphics or video implementation.

## 1. Purpose

Visual processing shapes how the imagery of a project is perceived.

Color grading — adjusting brightness, contrast, saturation, hue, and gamma to establish a mood or a
consistent look.

    Crops and aspect — framing and sizing imagery so it fits the intended composition.

Background handling — separating, softening, or removing a background so foreground content reads
clearly.

Brand and presets — applying a consistent, reusable look across a project or a library of projects.

In every case, OVML records what treatment is wanted, not how it is rendered.

## 2. Declarative and Non-Destructive

Visual processing is declarative and non-destructive.

A processing instruction describes the desired transformation of a source. It does not modify the
underlying image or video file. The same source can be processed differently in different uses, and
the original visual asset remains intact.

This is the same principle as audio processing and the general OVML model. A preset is a description
of intent; a Player or renderer interprets the description and produces the resulting image or
frame. The document never replaces or damages the source it references.

## 3. Where Processing Applies

Visual processing can be associated with a project at several levels.

    Scene — the look of a particular scene, guided by its atmosphere and color.

Character — a processing look applied to a character's imagery. A character may reference a video or
image preset through videoProcessorId and imageProcessorId, along with the corresponding name and
file attributes.

Element — a particular media element's processing, referenced through the processing attribute of a
media element:

```xml
        <video
            src="forest"
            processing="cinematic-dark" />
```

The processing preset is separate from the media asset, so the same asset can carry different looks
in different uses.

## 4. Processing Presets

A preset is a standalone XML document describing a sequence of processing operations. For video, a
preset uses <video_processing> as its root element and carries an id and a name:

```xml
    <video_processing id="video_guard_torch" name="Gate Guard Torchlight">
        ...
    </video_processing>
```

The standard reserves the concept of image-processing presets, but no normative image preset format
is currently documented in OVML 2.2. When a concrete format is established, it will follow the same
model.

A preset may be included in an OVMZ package under presets/video/ (and presets/image/ when defined),
or referenced externally. The Standard defines the preset document format; the Player determines how
an external preset is located, retrieved, validated, and executed.

## 5. Operations and Chaining

As with audio, a visual preset contains a list of operations evaluated in document order.

For example:

```xml
    <video_processing id="example" name="Example">
        <color enabled="true">
            ...
        </color>

        <blur enabled="true">
            ...
        </blur>

        <fade enabled="true">
            ...
        </fade>

        <convert>
            ...
        </convert>
    </video_processing>
```

This represents a chain:

```text
    Source
      ↓
    Color
      ↓
    Blur
      ↓
    Fade
      ↓
    Convert
      ↓
    Output
```

Operations that support optional activation use the enabled attribute; a disabled operation does not
participate in processing.

The current OVML 2.2 video vocabulary includes color, blur, sharpen, grayscale, sepia, grain,
vignette, invert, glow, lens flare, fade, overlay, and convert. These map to familiar image and
video concepts. Additional operations may be added when their XML representation and semantics are
formally established.

## 6. Brand and Presets

Because processing is expressed as reusable presets, a consistent look can be encapsulated once and
applied many times.

A brand preset — a defined color grade, framing, and background treatment — can be referenced from
scenes, characters, and media elements across an entire project. Changing the brand preset changes
the look of every element that references it, without editing each element. This makes visual
processing a natural carrier of brand identity.

The scene's atmosphere and color may also be used as hints when selecting visual processing presets,
guiding a consistent look that matches the creative intent of the scene.

## 7. Runtime Interpretation

A preset describes processing intent and parameters. It does not require a particular graphics
library, video library, codec library, operating system, or hardware platform.

A Player or renderer may implement the same operation using different underlying technologies —
scaling, filtering, shaders, or hardware-accelerated pipelines — while preserving the semantics
defined by the preset. A renderer that cannot fully realize an effect should preserve the semantics
it supports and clearly define what it cannot do.

## 8. Summary

Visual processing lets OVML describe the intended look of its imagery.

    It is declarative and non-destructive, describing intent without editing files.

    It applies at scene, character, and element levels.

    It is expressed as presets of chained color, effect, and conversion operations.

    It is a natural vehicle for brand and reusable looks.

    It is implementation-independent, so the same preset works across renderers.

The runtime interprets the processing description and produces the imagery; the document preserves
the source and the intent.

See: [`presets/video.md`](../presets/video.md)
See: [`presets/README.md`](../presets/README.md)
See: [`concepts/media-layers.md`](media-layers.md)
