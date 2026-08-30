# OVML Presets

**OpenVML — Open Voice Markup Language** is an open, XML-based standard for describing structured audiovisual content.

This section defines the OVML processing preset model.

A **preset** is a standalone XML document that describes a sequence of processing operations for a particular media type.

Presets are independent from the main content document and may be referenced by an OVML project.

## Preset Types

OVML processing presets are defined separately for different media types.

| Preset type | Root element         | Documentation          |
| ----------- | -------------------- | ---------------------- |
| Audio       | `<audio_processing>` | [`audio.md`](audio.md) |
| Video       | `<video_processing>` | [`video.md`](video.md) |
| Image       | `<image_processing>` | Not yet defined        |

The Standard reserves the concept of image processing presets, but no normative image preset format is currently documented in OVML 2.2.

The image preset specification will be added when a concrete format has been established.

## Standalone Preset Documents

A preset is a complete XML document.

It is not an XML fragment intended to be inserted into the main OVML document.

For example:

```
<audio_processing id="narrator_classic" name="Narrator Classic Warm">
    ...
</audio_processing>
```

The file normally uses the `.ovml` extension.

## Preset Directory Structure

Presets included in an OVMZ project may be organized as:

```
project.ovmz/
└── presets/
    ├── audio/
    │   └── <preset-id>.ovml
    └── video/
        └── <preset-id>.ovml
```

For example:

```
presets/
├── audio/
│   ├── baron_officer_crisp.ovml
│   ├── narrator_classic.ovml
│   └── voice_husky_drunk.ovml
└── video/
    ├── video_background_soft_blur_light.ovml
    ├── video_guard_torch.ovml
    └── video_mystic_veil.ovml
```

## Preset Identity

Every preset has an `id` and a human-readable `name`.

Example:

```
<audio_processing
    id="baron_officer_crisp"
    name="Baron Guard Officer">
```

### `id`

Machine-readable identifier of the preset.

### `name`

Human-readable name displayed by authoring applications and other compatible tools.

## Processing Chain

A preset contains processing operations.

Operations are normally evaluated in document order.

For example:

```
<audio_processing id="example" name="Example">
    <eq enabled="true">
        ...
    </eq>

    <compressor enabled="true">
        ...
    </compressor>

    <gain enabled="true">
        ...
    </gain>

    <convert>
        ...
    </convert>
</audio_processing>
```

The conceptual processing chain is:

```
Source
  ↓
EQ
  ↓
Compressor
  ↓
Gain
  ↓
Convert
  ↓
Output
```

The exact runtime implementation is determined by the Player.

## Enabled Operations

Processing operations that support optional activation use:

```
enabled="true"
```

or:

```
enabled="false"
```

An operation with `enabled="false"` is disabled and does not participate in processing.

Example:

```
<sharpen enabled="false">
    <amount>0</amount>
</sharpen>
```

## Audio Processing

The currently established OVML 2.2 audio preset vocabulary is documented in [`audio.md`](audio.md).

The currently implemented preset examples use:

* EQ;
* compressor;
* delay;
* reverb;
* chorus;
* gain;
* convert.

Additional audio operations may be introduced as their XML representation becomes formally defined.

## Video Processing

The currently established OVML 2.2 video preset vocabulary is documented in [`video.md`](video.md).

The currently implemented preset examples use:

* color;
* blur;
* sharpen;
* grayscale;
* sepia;
* grain;
* vignette;
* invert;
* glow;
* lens flare;
* fade;
* overlay;
* convert.

Additional video operations may be introduced as their XML representation becomes formally defined.

## Presets and the Main OVML Document

A preset may be associated with content in the main OVML document.

The reference identifies the processing configuration while the preset remains an independent document.

Example:

```
<character
    id="vestfal"
    name="Вестфаль"
    audioProcessorId="narrator_classic"
    audioProcessorFile="presets/audio/narrator_classic.ovml" />
```

The exact reference attributes belong to the corresponding content model.

## Presets and OVMZ

An OVMZ package may contain the presets required by the project:

```
project.ovmz/
├── content.ovml
├── resources/
├── presets/
│   ├── audio/
│   └── video/
└── ...
```

This allows processing definitions to travel together with the project.

## External Presets

A project may reference a preset outside the local package when the applicable project form and Player support external resources.

The Standard defines the preset document format.

The Player determines how an external preset is located, retrieved, validated, and executed.

## Implementation Independence

A preset describes processing intent and parameters.

It does not require a particular:

* audio library;
* video library;
* DSP implementation;
* codec library;
* operating system;
* hardware platform.

A Player may implement the same operation using different underlying technologies while preserving the semantics defined by the preset.

## Current Standard Scope

OVML 2.2 documents only processing formats that have an established XML representation.

The Standard does not invent XML syntax merely to provide a complete list of possible effects.

This distinction is intentional:

> **A documented operation is part of the language. An undocumented operation is not assumed to have a standardized XML representation.**

---

**OpenVML — Open Voice Markup Language**

**OVML Standard 2.2**
