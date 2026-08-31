# Audio Processing

**OpenVML Standard 2.2**

This document explains the conceptual model of audio processing in OVML: its purpose, why it is
declarative rather than destructive, where it applies, and how operations chain together.

Audio processing is how an OVML project describes the intended flavor of its sound — without
committing to any particular audio library or DSP implementation.

## 1. Purpose

Audio processing serves two broad goals.

Post-production flavor — shaping how speech and sound are heard. A narrator may sound warmer, a
character huskier, a distant voice more reverberant. This is part of the creative direction of the
work.

Accessibility and clarity — making speech easier to hear. Leveling, equalization, and other
adjustments can improve comprehension in noisy environments or for particular listeners.

In both cases, OVML records what processing is wanted, not how it is implemented.

## 2. Declarative, Not Destructive

Audio processing in OVML is declarative and non-destructive.

A processing instruction describes the desired transformation of a source. It does not modify the
underlying asset. The same source can be processed differently in different uses, and the original
audio remains intact and reusable.

This mirrors the general OVML principle. A preset is a description of intent; a Player or audio
pipeline interprets the description and produces the resulting signal. The document never destroys
the source it references.

## 3. Where Processing Applies

Audio processing can be associated with a project at several levels. The standard expresses which
processing is wanted, and the runtime decides where in its pipeline the processing is applied.

    Master — the overall sound of the project.

    Scene — the sound within a particular scene or atmosphere.

Character — the voice of a character. A character may reference an audio-processing preset through
audioProcessorId, audioProcessorName, and audioProcessorFile:

```xml
        <character
            id="vestfal"
            name="Vestfal"
            audioProcessorId="preset_1"
            audioProcessorName="Warm Voice"
            audioProcessorFile="presets/audio/WarmVoice.ovml" />
```

    Track — a particular media element, such as a background ambience loop or a sound effect.

The processing preset is separate from the voice declaration. A character's voice and its processing
are described independently, so either can change without the other.

## 4. Processing Presets

A preset is a standalone XML document that describes a sequence of processing operations. It is
independent of the main content document and may be referenced by a project.

For audio, a preset uses <audio_processing> as its root element and carries an id and a name:

```xml
    <audio_processing id="narrator_classic" name="Narrator Classic Warm">
        ...
    </audio_processing>
```

A preset may be included in an OVMZ package under presets/audio/, or referenced externally where the
Player supports it. The Standard defines the preset document format; the Player determines how an
external preset is located, retrieved, validated, and executed.

## 5. Chaining Operations

A preset contains a list of operations. Operations are normally evaluated in the order in which they
appear in the document.

For example:

```xml
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

This represents a chain:

```text
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

Each operation feeds the next. Operations that support optional activation use the enabled
attribute; an operation with enabled="false" does not participate in processing.

## 6. Operations and Vocabulary

The current OVML 2.2 audio vocabulary includes operations with a concrete XML representation: eq,
compressor, delay, reverb, chorus, gain, and convert. These map to familiar audio concepts:
equalization, dynamic-range compression, delay, reverberation, chorus/modulation, level adjustment,
and output conversion.

The OVML 2.2 preset examples demonstrate these operations in combination. Additional
audio-processing operations may be added to the standard when their XML representation and semantics
have been formally established; OVML does not invent syntax merely to provide a complete list of
possible effects.

## 7. Implementation Independence

A preset describes processing intent and parameters. It does not require a particular audio library,
DSP implementation, codec library, operating system, or hardware platform.

A Player may implement the same operation using different underlying technologies while preserving
the semantics defined by the preset. This is why the same preset can be honored by different Players
and rendering pipelines.

## 8. Summary

Audio processing lets OVML describe the intended character of a project's sound.

    It is declarative and non-destructive, describing intent rather than editing files.

    It applies at master, scene, character, and track levels.

    It is expressed as presets of chained operations.

    It is implementation-independent, so the same preset works across Players.

The runtime interprets the processing description and produces the sound; the document preserves the
source and the intent.

See: presets/audio.md
See: reference/tts.md
See: concepts/characters-and-voices.md
