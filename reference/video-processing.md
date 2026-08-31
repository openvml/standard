> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# `<video_processing>` — Video Processing

**OVML Standard 2.2**

## 1. Purpose

The `<video_processing>` element describes video processing applied to video material.

It supports color correction, effects, transformations, overlays, and output conversion.

Processing directives are declarative.

The document describes the intended processing. The Player or rendering implementation determines
how the processing is executed on the target platform.

## 2. Structure

The general structure is:

```xml
    <video_processing id="my_video_processor" name="Warm Vintage">

        <color enabled="true">
            <brightness>0.1</brightness>
            <contrast>0.15</contrast>
            <saturation>0.8</saturation>
            <hue>5</hue>
            <gamma>1.1</gamma>
        </color>

        <blur enabled="false">
            <radius>5</radius>
        </blur>

        <overlay enabled="false">
            <src>asset_logo</src>
            <opacity>0.8</opacity>
            <x>0.95</x>
            <y>0.05</y>
        </overlay>

        <convert>
            <format>mp4</format>
            <codec>h264</codec>
            <quality>high</quality>
        </convert>

    </video_processing>
```

Each child element is a processing directive.

The `<video_processing>` element itself is the container.

## 3. Container Attributes

Attribute	Type	Description
id	ID	Unique identifier of the processing definition
name	string	Human-readable name of the processing definition
enabled	boolean	Enables or disables the whole processing definition

Example:

```xml
    <video_processing id="my_video_processor" name="Warm Vintage">
        ...
    </video_processing>
```

When `enabled` is present and false, the processing definition does not participate in the
processing chain.

## 4. Processing Directives

A processing directive is a child element of `<video_processing>`.

Each directive describes one processing operation and its parameters.

A directive MAY be declared with:

- type;
- target;
- enabled;
- parameters.

type

Identifies the kind of processing operation.

In the XML form, the directive element name is the type.

target

Identifies the specific element or layer the directive addresses.

When absent, the directive applies to the whole material of its scope.

enabled

Activates or deactivates the directive.

A directive with enabled="false" does not participate in processing.

parameters

The effect-specific parameters of the directive.

Parameters are declarative hints.

The exact interpretation depends on the selected rendering engine.

## 5. Directive Types

The canonical processing directives are:

Element	Type	Description
color	color_grading	Color correction (brightness, contrast, saturation, hue, gamma)
grayscale	grayscale	Grayscale conversion
invert	invert	Color inversion
sepia	sepia	Sepia toning
blur	blur	Gaussian blur
sharpen	sharpen	Sharpening
crop	crop	Region cropping
resize	scale	Scaling / resolution change
rotate	rotate	Rotation
flip	flip	Horizontal / vertical mirroring
speed	speed	Playback speed
fade	fade	Fade in / fade out
overlay	position_offsets	Image overlay and positioning
chroma_key	background_removal	Background removal by chroma key
text	subtitle_style	Text, caption, and subtitle styling
convert	convert	Output conversion

The type vocabulary also includes the semantic intents aspect_ratio, canvas_zoom_pan,
container_color, and caption_enable.

These are resolved through the corresponding directives:

Type	Semantic intent
aspect_ratio	Expressed through crop and resize (fit, fill, stretch)
canvas_zoom_pan	Canvas motion; expressed through resize and camera instructions
container_color	Canvas / container background color behind the media, expressed through fade color or scene color
caption_enable	Whether text is rendered, expressed through the enabled state of text

Processing directives are declarative. The runtime interprets them.

### 6. color

The color directive applies color correction.

```xml
    <color enabled="true">
        <brightness>0.1</brightness>
        <contrast>0.15</contrast>
        <saturation>0.8</saturation>
        <hue>5</hue>
        <gamma>1.1</gamma>
    </color>
```

Parameter	Description	Range
brightness	Brightness adjustment	-1 — +1
contrast	Contrast adjustment	-1 — +1
saturation	Saturation adjustment	-1 — +1
hue	Hue rotation	-180 — +180
gamma	Gamma correction	0.1 — 3.0

### 7. grayscale

The grayscale directive converts the material to black and white.

```xml
    <grayscale enabled="false">
        <intensity>1.0</intensity>
    </grayscale>
```

Parameter	Description	Range
intensity	Grayscale intensity	0 — 1

### 8. invert

The invert directive inverts the colors.

```xml
    <invert enabled="false">
        <intensity>1.0</intensity>
    </invert>
```

Parameter	Description	Range
intensity	Inversion intensity	0 — 1

### 9. sepia

The sepia directive applies a sepia tone.

```xml
    <sepia enabled="false">
        <intensity>0.7</intensity>
    </sepia>
```

Parameter	Description	Range
intensity	Sepia intensity	0 — 1

### 10. blur

The blur directive applies a gaussian blur.

```xml
    <blur enabled="false">
        <radius>5</radius>
    </blur>
```

Parameter	Description	Range
radius	Blur radius	0 — 50

### 11. sharpen

The sharpen directive increases perceived sharpness.

```xml
    <sharpen enabled="false">
        <amount>1.5</amount>
    </sharpen>
```

Parameter	Description	Range
amount	Sharpening amount	0 — 5

### 12. rotate

The rotate directive rotates the material.

```xml
    <rotate enabled="false">
        <angle>90</angle>
    </rotate>
```

Angles may be 90, 180, or 270 degrees.

Parameter	Description	Range
angle	Rotation angle	-180 — +180 degrees

### 13. flip

The flip directive mirrors the material horizontally and/or vertically.

```xml
    <flip enabled="false">
        <horizontal>true</horizontal>
        <vertical>false</vertical>
    </flip>
```

Parameter	Description	Values
horizontal	Horizontal mirroring	true / false
vertical	Vertical mirroring	true / false

### 14. crop

The crop directive crops the material to a region.

Coordinates and sizes are expressed as fractions of the source dimensions.

```xml
    <crop enabled="false">
        <x>0.1</x>
        <y>0.1</y>
        <width>0.8</width>
        <height>0.8</height>
    </crop>
```

Parameter	Description	Range
x	Left offset	0 — 1
y	Top offset	0 — 1
width	Crop width	0.1 — 1
height	Crop height	0.1 — 1

### 15. resize

The resize directive changes the size of the material.

```xml
    <resize enabled="false">
        <width>1920</width>
        <height>1080</height>
        <mode>fit</mode>
    </resize>
```

Parameter	Description	Values
width	Target width	160 — 3840 px
height	Target height	120 — 2160 px
mode	Fitting mode	fit, fill, stretch

fit preserves the aspect ratio inside the target bounds.

fill fills the target bounds while preserving the aspect ratio.

stretch fits the material to the target bounds without preserving the aspect ratio.

The resize directive expresses the aspect_ratio and scale semantics.

### 16. speed

The speed directive changes the playback speed.

```xml
    <speed enabled="false">
        <factor>1.0</factor>
    </speed>
```

Parameter	Description	Range
factor	Speed factor	0.25 — 4.0

### 17. fade

The fade directive applies fade in and fade out.

```xml
    <fade enabled="false">
        <in_duration>1.0</in_duration>
        <out_duration>2.0</out_duration>
    </fade>
```

Parameter	Description	Range
in_duration	Fade-in duration	0 — 10 s
out_duration	Fade-out duration	0 — 10 s

### 18. overlay

The overlay directive composites another resource over the material.

The x and y values position the overlay; they express the position_offsets semantics.

```xml
    <overlay enabled="false">
        <src>asset_logo</src>
        <opacity>0.8</opacity>
        <x>0.95</x>
        <y>0.05</y>
    </overlay>
```

Parameter	Description	Range
src	Asset identifier of the overlay resource	asset reference
opacity	Overlay opacity	0 — 1
x	Horizontal position	0 — 1 (1 = right edge)
y	Vertical position	0 — 1 (1 = bottom edge)

### 19. chroma_key

The chroma_key directive removes a background color.

It expresses the background_removal semantics.

```xml
    <chroma_key enabled="false">
        <similarity>0.4</similarity>
        <color>#00FF00</color>
    </chroma_key>
```

Parameter	Description	Range
similarity	Color similarity threshold	0 — 1
color	Color to remove	#RRGGBB

### 20. text

The text directive renders text over the material.

It expresses the subtitle_style and caption_enable semantics.

```xml
    <text enabled="false">
        <content>Sample Text</content>
        <size>24</size>
        <color>#FFFFFF</color>
        <font>Arial</font>
        <x>0.5</x>
        <y>0.9</y>
    </text>
```

Parameter	Description	Range
content	Text content	text
size	Font size	8 — 200
color	Text color	#RRGGBB
font	Font family	implementation-defined
x	Horizontal position	0 — 1
y	Vertical position	0 — 1

### 21. convert

The convert directive describes the requested output format.

```xml
    <convert>
        <format>mp4</format>
        <codec>h264</codec>
        <quality>high</quality>
    </convert>
```

Parameter	Description	Allowed values
format	Container format	mp4, webm, mov, avi
codec	Video codec	h264, h265, vp9, av1
quality	Encoding quality	low, medium, high

## 22. Application

Processing for a video element may be declared at several scopes.

The scopes form an inheritance chain:

```text
    Project (<settings>)
              ↓
        Character
              ↓
    Scene / block / media element
```

A more specific declaration overrides or supplements the more general one.

## 23. Project-Level Declaration

A project MAY declare default video processing in its `<settings>` section.

For example:

```xml
    <settings>

        <video_processing>
            <color enabled="true">
                <contrast>0.05</contrast>
                <saturation>1.1</saturation>
            </color>
        </video_processing>

    </settings>
```

Project-level processing applies as the default to the project's video material unless a more
specific declaration overrides it.

## 24. Character-Level Declaration

A character MAY reference a video-processing preset.

```xml
    <character
        id="vestfal"
        name="Vestfal"
        videoProcessorId="video_01"
        videoProcessorName="Cinematic"
        videoProcessorFile="presets/video/Cinematic.ovml" />
```

See: reference/character.md

## 25. Media and Block-Level Declaration

A video media element MAY reference a processing preset by the `processing` attribute:

```xml
    <video
        src="my_video"
        action="play"
        processing="my_video_processor" />
```

Processing may also be declared inline as child directives of the media element:

```xml
    <video src="background" action="play">

        <color>
            <brightness>0.2</brightness>
            <contrast>0.1</contrast>
        </color>

        <blur>
            <radius>3</radius>
        </blur>

    </video>
```

Inline directives apply only to that media element.

See: reference/media.md

## 26. Scene-Level Declaration

A scene MAY provide context for processing preset selection.

The scene provides context; it does not directly execute a preset unless an explicit processing
declaration or character configuration specifies one.

See: reference/scene.md

## 27. Layer-Specific Instantiation

Different visual layers may receive different processing.

For example, a background layer may be blurred while the foreground layer remains sharp, or a
foreground layer may receive a color grade that the background does not.

Layer-specific instantiation is resolved by the Player.

## 28. Design Principle

<video_processing> describes the intended video processing.

It does not require a particular:

- video library;
- rendering engine;
- FFmpeg pipeline;
- codec library;
- operating system;
- hardware platform.

A Player MAY implement the same operation using different underlying technologies while preserving
the semantics described by the declaration.

Processing directives are declarative. The runtime interprets them.

## 29. Related Documents

See: reference/media.md
See: reference/character.md
See: reference/cast.md
See: reference/audio-processing.md
See: reference/image-processing.md
See: ../presets/video.md
See: ../presets/README.md
