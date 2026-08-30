> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Layers — Background, Foreground, Overlay

**OVML Standard 2.2**

## 1. Purpose

The layer attribute defines the intended visual layer of a media element.

OVML defines the following layer values:

- background;
- foreground;
- overlay.

Layers describe the intended visual composition.

They do not prescribe a particular rendering implementation.

The actual compositing pipeline is determined by the Player or renderer.

## 2. The Layer Model

Visual content is conceptually ordered from back to front:

Overlay
    ↑
Foreground
    ↑
Background

The base of the composition is the background layer.

Content that should appear in front of the background uses the foreground layer.

Content that should appear above the foreground — such as subtitles, captions, title cards, and UI elements — uses the overlay layer.

## 3. Layer Values

Value	Description
background	The base visual layer
foreground	Content above the background layer
overlay	Content composited above the foreground layer

Example:

<video
`` `src="city-background"
layer="background" />
`` `
<img
`` `src="character"
layer="foreground"
sizePercent="60" />
`` `
The intended conceptual ordering is:

Foreground
    ↑
Content
    ↑
Background

For example:

<scene atmosphere="night city">

`` `<video
    src="city-background"
    layer="background" />

<img
    src="character"
    layer="foreground"
    sizePercent="60" />

<p>
    <line char="alex">
        Look at the lights.
    </line>
</p>
`` `
</scene>

The video forms the base.

The character image appears in front of it.

The line text may be rendered above the composition as an overlay caption when the Player presents subtitles.

## 4. Overlay

The overlay layer is used for content that sits above the foreground.

Typical overlay content includes:

subtitles and captions;
title cards;
lower thirds;
countdowns;
watermarks;
interface overlays.

Overlay content is intended to be composited last, above the main visual content.

The exact rendering and opacity handling are implementation-dependent.

## 5. Multiple Background Elements

Several media elements may share the same layer.

For example, multiple audio elements may use the background layer to form an ambience bed:

<audio
`` `src="rain"
layer="background"
volume="0.3"
loop="true" />
`` `
<audio
`` `src="wind"
layer="background"
volume="0.2"
loop="true" />
`` `
The layer expresses the intended grouping, not a strict z-order between elements on the same layer.

## 6. Layering and Timing

Timing and visual layering are independent concepts.

A media element defines:

when it is active (timing);
where it is placed (layer).

For example:

<video
`` `src="background.mp4"
layer="background"
startMode="absolute"
startTime="0"
duration="30" />
`` `
<img
`` `src="character.png"
layer="foreground"
startMode="absolute"
startTime="5"
duration="10" />
`` `
The first element defines when the background video is active.

The second defines when the foreground image is active.

The layer attribute determines visual composition.

Timing determines temporal composition.

## 7. Reusing an Asset Across Layers

The same asset may be used on different layers with different presentation settings.

<img
`` `src="castle"
layer="background"
sizePercent="100" />
`` `
<img
`` `src="castle"
layer="foreground"
sizePercent="40" />
`` `
The underlying asset is unchanged.

Each media element describes an independent use.

## 8. Audio Layers

Audio elements may also use the layer attribute to express the intended depth of the mix.

background audio — ambience, background music;
foreground audio — speech, prominent sound effects;
overlay audio — continuous interface or notification-like audio.

The layer value is an intent hint.

The actual mixing balance is determined by the Player or audio processing pipeline.

## 9. Implementation Flexibility

An implementation MAY support additional internal layers.

For example, a rendering system may internally resolve background, foreground, and overlay into a finer-grain compositing stack.

The standard's three values describe the portable intent.

Subtitle and caption content, for example, is normally composited as an overlay regardless of the renderer used.

See: reference/subtitle.md

## 10. Validation

An OVML validator SHOULD verify:

- layer contains an allowed value or an implementation-defined value;
- boolean and numeric presentation attributes contain valid values;
- the media element is properly formed.

The validator does not determine whether a particular layer value produces a visually desirable result in a given Player.

## 11. Design Principle

OVML describes the intended visual composition.

It does not describe the compositing implementation.

Layer
   → describes where content belongs

Player / Renderer
   → determines how layers are composed

This separation allows the same media elements to be used by different renderers, platforms, and display systems while preserving their intended visual relationship.

## 12. Related Documents

See: reference/media.md
See: reference/subtitle.md
See: reference/timing.md
See: reference/scene.md
See: concepts/timeline-and-blocks.md