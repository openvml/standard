# OVML Video Processing Presets

**OpenVML — Open Voice Markup Language** is an open, XML-based standard for describing structured
audiovisual content.

This document defines the currently established video processing preset format in OVML 2.2.

## `video_processing`

A video processing preset uses `<video_processing>` as its root element.

```xml
<video_processing id="preset_id" name="Preset Name">
    ...
</video_processing>
```

### Attributes

| Attribute | Description                        |
| --------- | ---------------------------------- |
| `id`      | Machine-readable preset identifier |
| `name`    | Human-readable preset name         |

## Processing Order

Video operations are normally evaluated in document order.

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

The conceptual chain is:

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
```

## `enabled`

Operations that support optional activation use:

enabled="true"

or:

enabled="false"

A disabled operation does not participate in processing.

## Color

The `<color>` element controls basic image characteristics.

```xml
<color enabled="true">
    <brightness>-0.08</brightness>
    <contrast>0.2</contrast>
    <saturation>-0.15</saturation>
    <hue>-10</hue>
    <gamma>0.9</gamma>
</color>
```

### Parameters

| Element        | Description           |
| -------------- | --------------------- |
| `<brightness>` | Brightness adjustment |
| `<contrast>`   | Contrast adjustment   |
| `<saturation>` | Saturation adjustment |
| `<hue>`        | Hue adjustment        |
| `<gamma>`      | Gamma adjustment      |

## Blur

The `<blur>` element applies blur.

```xml
<blur enabled="true">
    <radius>12</radius>
</blur>
```

### `<radius>`

Defines the blur radius.

## Sharpen

The `<sharpen>` element controls image/video sharpening.

```xml
<sharpen enabled="false">
    <amount>0</amount>
</sharpen>
```

### `<amount>`

Defines sharpening intensity.

## Grayscale

The `<grayscale>` element converts the image toward grayscale.

```xml
<grayscale enabled="false">
    <intensity>0</intensity>
</grayscale>
```

### `<intensity>`

Defines grayscale intensity.

## Sepia

The `<sepia>` element applies a sepia effect.

```xml
<sepia enabled="true">
    <intensity>0.25</intensity>
</sepia>
```

## Grain

The `<grain>` element adds film-like grain.

```xml
<grain enabled="true">
    <intensity>0.1</intensity>
</grain>
```

### `<intensity>`

Defines grain intensity.

## Vignette

The `<vignette>` element applies a vignette effect.

```xml
<vignette enabled="true">
    <amount>0.3</amount>
</vignette>
```

### `<amount>`

Defines vignette strength.

## Invert

The `<invert>` element controls color inversion.

```xml
<invert enabled="false">
    <intensity>0</intensity>
</invert>
```

### `<intensity>`

Defines the intensity of the inversion effect.

## Glow

The `<glow>` element applies a glow effect.

```xml
<glow enabled="true">
    <intensity>0.18</intensity>
</glow>
```

## Lens Flare

The `<lens_flare>` element applies a lens flare effect.

```xml
<lens_flare enabled="true">
    <intensity>0.2</intensity>
</lens_flare>
```

## Fade

The `<fade>` element defines video fade-in and fade-out durations.

```xml
<fade enabled="true">
    <in_duration>0.3</in_duration>
    <out_duration>0.4</out_duration>
</fade>
```

### Parameters

| Element          | Description       |
| ---------------- | ----------------- |
| `<in_duration>`  | Fade-in duration  |
| `<out_duration>` | Fade-out duration |

## Overlay

The `<overlay>` element defines an overlay operation.

```xml
<overlay enabled="true">
    <opacity>0.25</opacity>
    <x>0</x>
    <y>0</y>
</overlay>
```

### Parameters

| Element     | Description         |
| ----------- | ------------------- |
| `<opacity>` | Overlay opacity     |
| `<x>`       | Horizontal position |
| `<y>`       | Vertical position   |

The exact overlay source relationship is determined by the applicable media composition model.

## Convert

The `<convert>` element defines output video conversion parameters.

```xml
<convert>
    <format>mp4</format>
    <codec>h264</codec>
    <bitrate>9000</bitrate>
    <resolution>1920x1080</resolution>
</convert>
```

### Parameters

| Element        | Description             |
| -------------- | ----------------------- |
| `<format>`     | Output container format |
| `<codec>`      | Video codec             |
| `<bitrate>`    | Output bitrate          |
| `<resolution>` | Output resolution       |

Not every parameter is required in every preset.

## Complete Example: Background Soft Blur Light

```xml
<video_processing id="video_background_soft_blur_light" name="Background Soft Blur Light">
    <color enabled="true">
        <brightness>-0.05</brightness>
        <contrast>-0.06</contrast>
        <saturation>-0.45</saturation>
        <hue>0</hue>
        <gamma>0.95</gamma>
    </color>

    <blur enabled="true">
        <radius>12</radius>
    </blur>

    <sharpen enabled="false">
        <amount>0</amount>
    </sharpen>

    <grayscale enabled="false">
        <intensity>0</intensity>
    </grayscale>

    <sepia enabled="false">
        <intensity>0</intensity>
    </sepia>

    <fade enabled="true">
        <in_duration>0.3</in_duration>
        <out_duration>0.4</out_duration>
    </fade>

    <overlay enabled="true">
        <opacity>0.25</opacity>
        <x>0</x>
        <y>0</y>
    </overlay>

    <convert>
        <format>mp4</format>
        <codec>h264</codec>
    </convert>
</video_processing>
```

## Complete Example: Gate Guard Torchlight

```xml
<video_processing id="video_guard_torch" name="Gate Guard Torchlight">
    <color enabled="true">
        <brightness>-0.08</brightness>
        <contrast>0.2</contrast>
        <saturation>-0.15</saturation>
        <hue>-10</hue>
        <gamma>0.9</gamma>
    </color>

    <sepia enabled="true">
        <intensity>0.25</intensity>
    </sepia>

    <grain enabled="true">
        <intensity>0.1</intensity>
    </grain>

    <vignette enabled="true">
        <amount>0.3</amount>
    </vignette>

    <blur enabled="false">
        <radius>0</radius>
    </blur>

    <convert>
        <format>mp4</format>
        <codec>h264</codec>
        <bitrate>9000</bitrate>
        <resolution>1920x1080</resolution>
    </convert>
</video_processing>
```

## Complete Example: Mystic Veil

```xml
<video_processing id="video_mystic_veil" name="Mystic Veil">
    <color enabled="true">
        <brightness>-0.05</brightness>
        <contrast>0.08</contrast>
        <saturation>0.2</saturation>
        <hue>-5</hue>
        <gamma>1.1</gamma>
    </color>

    <blur enabled="true">
        <radius>4</radius>
    </blur>

    <grayscale enabled="false">
        <intensity>0</intensity>
    </grayscale>

    <invert enabled="false">
        <intensity>0</intensity>
    </invert>

    <glow enabled="true">
        <intensity>0.18</intensity>
    </glow>

    <lens_flare enabled="true">
        <intensity>0.2</intensity>
    </lens_flare>

    <convert>
        <format>mp4</format>
        <codec>h265</codec>
        <bitrate>14000</bitrate>
        <resolution>1920x1080</resolution>
    </convert>
</video_processing>
```

## Current Video Vocabulary

The following video operations have a concrete XML representation in the current OVML 2.2 preset
examples:

* `color`
* `blur`
* `sharpen`
* `grayscale`
* `sepia`
* `grain`
* `vignette`
* `invert`
* `glow`
* `lens_flare`
* `fade`
* `overlay`
* `convert`

Other video-processing operations may be added to the standard when their XML representation and
semantics have been formally established.

---

**OpenVML — Open Voice Markup Language**

**OVML Standard 2.2**
