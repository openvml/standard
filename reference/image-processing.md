> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# `<image_processing>` — Image Processing

**OVML Standard 2.2**

1. Purpose

The `<image_processing>` element describes processing applied to still images (`<img>` / `<image>`).

It is the image counterpart of `<video_processing>`.

Image processing uses the same declarative mechanism as video processing, applied to static rather than temporal visual media.

An image has no intrinsic playback duration. Its duration is determined by the media element and the surrounding timeline.

See: reference/video-processing.md
See: reference/assets.md

2. Structure

The general structure is:

```<image_processing id="preset_img_bw" name="Black and White">

    <grayscale enabled="true">
        <intensity>1</intensity>
    </grayscale>

    <convert>
        <format>png</format>
        <quality>95</quality>
    </convert>

</image_processing>
`` `
Each child element is a processing directive.

The `<image_processing>` element itself is the container.

3. Container Attributes

Attribute	Type	Description
id	ID	Unique identifier of the processing definition
name	string	Human-readable name of the processing definition
enabled	boolean	Enables or disables the whole processing definition

Example:

`` `<image_processing id="preset_img_bw" name="Black and White">
    ...
</image_processing>
`` `
When `enabled` is present and false, the processing definition does not participate in the processing chain.

4. Processing Directives

A processing directive is a child element of `<image_processing>`.

Each directive describes one processing operation and its parameters.

A directive MAY be declared with:

type;
target;
enabled;
parameters.

type

Identifies the kind of processing operation.

In the XML form, the directive element name is the type.

target

Identifies the specific element or layer the directive addresses.

When absent, the directive applies to the whole image of its scope.

enabled

Activates or deactivates the directive.

A directive with enabled="false" does not participate in processing.

parameters

The effect-specific parameters of the directive.

Parameters are declarative hints.

The exact interpretation depends on the selected rendering implementation.

5. Directive Types

Image processing mirrors video processing.

The canonical processing directives are:

Element	Type	Semantics
color	color_grading	Brightness, contrast, saturation, hue, gamma
brightness	brightness	Brightness component of the color directive
contrast	contrast	Contrast component of the color directive
grayscale	grayscale	Grayscale conversion
invert	invert	Color inversion
sepia	sepia	Sepia toning
blur	blur	Gaussian blur
sharpen	sharpen	Sharpening
crop	crop	Region cropping
resize	scale	Scaling / resolution change
rotate	rotate	Rotation
flip	flip	Horizontal / vertical mirroring
overlay	position_offsets	Image overlay and positioning
overlay	watermark	Watermark / logo compositing
fade	fade	Fade in / fade out
convert	convert	Output conversion

The semantic intents aspect_ratio, background_removal, and container_color are shared with video processing:

Type	Semantic intent
aspect_ratio	Expressed through crop and resize (fit, fill, stretch)
background_removal	Background removal of an image, where supported by the runtime, through the same chroma-key / matting mechanism as video processing
container_color	Canvas / container background color behind the image, expressed through fade color or scene color

Whether a particular implementation supports a given operation for still images is a runtime capability.

Processing directives are declarative. The runtime interprets them.

6. color

The color directive applies color correction.

`` `<color enabled="true">
    <brightness>0.1</brightness>
    <contrast>0.15</contrast>
    <saturation>0.8</saturation>
    <hue>5</hue>
    <gamma>1.1</gamma>
</color>
`` `
Parameter	Description	Range
brightness	Brightness adjustment	-1 — +1
contrast	Contrast adjustment	-1 — +1
saturation	Saturation adjustment	-1 — +1
hue	Hue rotation	-180 — +180
gamma	Gamma correction	0.1 — 3.0

brightness and contrast express the semantics of the same-named types.

7. grayscale

The grayscale directive converts the image to black and white.

`` `<grayscale enabled="true">
    <intensity>1</intensity>
</grayscale>
`` `
Parameter	Description	Range
intensity	Grayscale intensity	0 — 1

8. invert

The invert directive inverts the colors.

`` `<invert enabled="false">
    <intensity>1.0</intensity>
</invert>
`` `
Parameter	Description	Range
intensity	Inversion intensity	0 — 1

9. sepia

The sepia directive applies a sepia tone.

`` `<sepia enabled="false">
    <intensity>0.7</intensity>
</sepia>
`` `
Parameter	Description	Range
intensity	Sepia intensity	0 — 1

10. blur

The blur directive applies a gaussian blur.

`` `<blur enabled="false">
    <radius>5</radius>
</blur>
`` `
Parameter	Description	Range
radius	Blur radius	0 — 50

11. sharpen

The sharpen directive increases perceived sharpness.

`` `<sharpen enabled="false">
    <amount>1.0</amount>
</sharpen>
`` `
Parameter	Description	Range
amount	Sharpening amount	0 — 5

12. rotate

The rotate directive rotates the image.

`` `<rotate enabled="false">
    <angle>90</angle>
</rotate>
`` `
Angles may be 90, 180, or 270 degrees.

Parameter	Description	Range
angle	Rotation angle	-180 — +180 degrees

13. flip

The flip directive mirrors the image horizontally and/or vertically.

`` `<flip
    enabled="true"
    horizontal="true"
    vertical="false" />
`` `
Parameter	Description	Values
horizontal	Horizontal mirroring	true / false
vertical	Vertical mirroring	true / false

14. crop

The crop directive crops the image to a region.

Coordinates and sizes are expressed as fractions of the source dimensions.

`` `<crop enabled="false">
    <x>0.1</x>
    <y>0.1</y>
    <width>0.8</width>
    <height>0.8</height>
</crop>
`` `
Parameter	Description	Range
x	Left offset	0 — 1
y	Top offset	0 — 1
width	Crop width	0.1 — 1
height	Crop height	0.1 — 1

15. resize

The resize directive changes the size of the image.

`` `<resize enabled="false">
    <width>1920</width>
    <height>1080</height>
    <keep_aspect>true</keep_aspect>
</resize>
`` `
Parameter	Description	Values
width	Target width	px
height	Target height	px
keep_aspect	Preserve aspect ratio	true / false

The resize directive expresses the scale semantics.

Formatting into a target aspect ratio uses the crop and resize directives together.

16. overlay

The overlay directive composites another image over the base image.

It may express a watermark or logo, and positions and sizes the composited image.

`` `<overlay enabled="true">
    <opacity>0.8</opacity>
    <x>0.05</x>
    <y>0.05</y>
    <width>0.3</width>
    <height>0.3</height>
    <mode>percent</mode>
</overlay>
`` `
Parameter	Description
opacity	Overlay opacity (0 — 1)
x	Horizontal position
y	Vertical position
width	Overlay width
height	Overlay height
mode	Position mode (percent)

The overlay directive expresses the position_offsets and watermark semantics.

17. fade

The fade directive applies fade in and fade out.

`` `<fade enabled="false">
    <in_duration>0.5</in_duration>
    <out_duration>0.5</out_duration>
</fade>
`` `
Parameter	Description	Range
in_duration	Fade-in duration	0 — 10 s
out_duration	Fade-out duration	0 — 10 s

18. convert

The convert directive describes the requested output format.

`` `<convert>
    <format>png</format>
    <quality>95</quality>
</convert>
`` `
Parameter	Description	Values
format	Container format	png, jpg, webp
quality	Encoding quality	0 — 100

19. Application

Processing for an image may be declared at several scopes.

The scopes form an inheritance chain:

`` `Project (<settings>)
          ↓
    Character
          ↓
Scene / block / media element
`` `
A more specific declaration overrides or supplements the more general one.

20. Project-Level Declaration

A project MAY declare default image processing in its `<settings>` section.

Project-level processing applies as the default to the project's image material unless a more specific declaration overrides it.

21. Character-Level Declaration

A character MAY reference an image-processing preset.

`` `<character
    id="vestfal"
    name="Vestfal"
    imageProcessorId="image_01"
    imageProcessorName="Dark Fantasy"
    imageProcessorFile="presets/image/DarkFantasy.ovml" />
`` `
See: reference/character.md

22. Media and Block-Level Declaration

An image media element MAY reference a processing preset by the `processing` attribute:

`` `<img
    src="portrait"
    processing="image_01" />
`` `
Processing may also be declared inline as child directives of the media element.

Inline directives apply only to that media element.

See: reference/media.md

23. Scene-Level Declaration

A scene MAY provide context for processing preset selection.

The scene provides context; it does not directly execute a preset unless an explicit processing declaration or character configuration specifies one.

See: reference/scene.md

24. Design Principle

<image_processing> describes the intended image processing.

It mirrors <video_processing> and shares its semantics for still images.

It does not require a particular:

image library;
rendering engine;
codec library;
operating system;
hardware platform.

A Player MAY implement the same operation using different underlying technologies while preserving the semantics described by the declaration.

Processing directives are declarative. The runtime interprets them.

25. Related Documents

See: reference/video-processing.md
See: reference/media.md
See: reference/character.md
See: reference/cast.md
See: reference/audio-processing.md
See: ../presets/README.md