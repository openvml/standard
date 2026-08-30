# Media Layers

**OpenVML Standard 2.2**

This document explains the conceptual layer system of OVML: how background, foreground, and overlay stack images and video within a scene, and how layer, timing, anchoring, and size combine to produce a visual composition.

Layers describe the intended visual composition. They do not prescribe a particular compositing implementation. The actual pipeline is determined by the Player or renderer.

1. Why Layers Exist

An OVML scene can combine several visual elements at once: a background video, a foreground image, a caption on top. Without a way to say which element belongs in front of which, the composition would be ambiguous.

The layer attribute expresses where a visual element belongs in depth. It is a statement of intent about visual stacking, separate from the element's timing.

2. The Layer Model

Visual content is conceptually ordered from back to front:

    Overlay
        ↑
    Foreground
        ↑
    Background

    Background — the base layer of the composition;

    Foreground — content that appears in front of the background;

    Overlay — content composited above the foreground, such as subtitles, captions, title cards, and interface elements.

For example:

```xml
<scene atmosphere="night city">

    <video
        src="city-background"
        layer="background" />

    <img
        src="character"
        layer="foreground"
        sizePercent="60" />

</scene>
```

The video forms the base. The character image appears in front of it.

3. Layer Versus Timing

Timing and visual layering are independent concepts.

A media element defines:

    when it is active — its timing;
    where it is placed — its layer.

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

The first element defines when the background video is active. The second defines when the foreground image is active. The layer attribute determines visual composition; timing determines temporal composition. The two can overlap in any way without conflicting.

4. Anchoring and Size

Beyond its layer, a visual element is positioned and sized within the composition.

    sizePercent requests a relative visual size. A value of 100 represents the full available reference size; 50 requests roughly half.

    grid attributes (gridRow, gridCol, gridRowSpan, gridColSpan) position the element within a visual grid.

For example:

```xml
<img
    src="castle"
    layer="foreground"
    sizePercent="40"
    gridRow="2"
    gridCol="3" />
```

The layer says which depth the element occupies. The size and grid say where, within that layer, the element sits and how large it is. These are separate declarative dimensions of the element.

5. Parallax and Depth

Because layers are depth expressions rather than rendering commands, a Player or renderer may interpret them in richer ways.

A renderer may treat the background, foreground, and overlay as a depth stack and apply parallax — moving elements at different depths by different amounts as the composition changes. A camera instruction may then be composed with these layers.

The layer is the portable intent; the renderer decides how much depth, parallax, or motion it can realize. An implementation should not claim to provide a depth effect the underlying renderer cannot support.

6. Multiple Elements on a Layer

Several media elements may share the same layer. For example, multiple audio elements may form an ambience bed:

```xml
<audio
    src="rain"
    layer="background"
    volume="0.3"
    loop="true" />

<audio
    src="wind"
    layer="background"
    volume="0.2"
    loop="true" />
```

The layer expresses the intended grouping, not a strict z-order between elements on the same layer. Audio elements may also use the layer attribute to express the intended depth of the mix — background ambience, foreground speech, overlay notification-style audio.

7. Reusing an Asset Across Layers

The same asset may be used on different layers with different presentation settings:

```xml
<img
    src="castle"
    layer="background"
    sizePercent="100" />

<img
    src="castle"
    layer="foreground"
    sizePercent="40" />
```

The underlying asset is unchanged. Each media element describes an independent use of the resource with its own layer, size, and timing.

8. How Layer, Timing, and Size Compose

A complete visual composition results from combining these declarative dimensions.

    Layer — where the element belongs in depth (background, foreground, overlay).

    Timing — when the element is active and how long it lasts.

    Size and position — how large it appears and where it sits.

The same way timing composes over time, layers compose in depth. A renderer takes the elements active at a given moment, orders them by layer, sizes and positions each one, and composites the result.

9. Summary

The layer system lets an OVML author describe a visual composition without describing the renderer.

    Background, foreground, and overlay stack visual content from back to front.

    Layer, timing, size, and position are independent declarative dimensions.

    Multiple elements may share a layer, and a single asset may be reused across layers.

    The layer is intent; the Player or renderer decides how depth and parallax are realized.

This separation allows the same media elements to be used by different renderers, platforms, and display systems while preserving their intended visual relationship.

See: reference/media.md
See: reference/layers.md
See: concepts/camera.md
See: concepts/timeline-and-blocks.md
