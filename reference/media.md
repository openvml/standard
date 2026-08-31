> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# Media Elements

**OVML Standard 2.2**

## 1. Purpose

OVML media elements describe how audiovisual resources are used at a particular point in the script.

The standard defines three primary media elements:

```xml
<video />
<img />
<audio />
```

A media element does not itself represent the underlying media file.

Instead:

```text
Asset
    │
    │ referenced by
    ▼
Media Element
    │
    ├── when to start
    ├── how long to play
    ├── where to place
    ├── how large to display
    ├── how loudly to play
    └── how to process
```

This distinction allows the same asset to be reused in different places and with different playback
parameters.

## 2. Media Asset vs. Media Element

A media asset is a resource such as:

- image;
- video;
- audio;
- animation;
- other supported media.

A media element is an instruction to use that resource in the OVML timeline.

For example:

```xml
<img src="forest" />
```

The value forest identifies the resource.

The `<img>` element additionally defines how that resource is used at this particular point in the
script.

For example:

```xml
<img
    src="forest"
    layer="background"
    sizePercent="100"
    startMode="absolute"
    startTime="0" />
```

The same asset may be used again:

```xml
<img
    src="forest"
    layer="foreground"
    sizePercent="40"
    startMode="absolute"
    startTime="15" />
```

The resource is the same.

The presentation instructions are different.

## 3. Supported Media Elements

OVML defines three primary media elements.

Element	Purpose
`<video>`	Video content
`<img>`	Still image content
`<audio>`	Audio content

Example:

```xml
<video src="intro-video" />
<img src="title-image" />
<audio src="background-music" />
```

### 4. src

The src attribute identifies the media resource.

Example:

```xml
<video src="intro-video" />
```

The value may represent different kinds of resource references depending on the project form and
resource system.

Possible forms include:

Asset ID
External URL
Package-relative path
Implementation-defined resource reference

For example:

```xml
<img src="forest-background" />
```

or:

```xml
<img src="https://example.com/images/forest.jpg" />
```

or, in a packaged project:

```xml
<img src="resources/images/forest.jpg" />
```

The exact interpretation of a non-URL identifier is determined by the project's asset/resource
system.

## 5. External Resources

A plain .ovml document MAY reference resources hosted externally.

Example:

```xml
<scene>

    <img
        src="https://example.com/images/forest.jpg"
        layer="background" />

    <audio
        src="https://example.com/audio/forest.mp3"
        layer="background"
        volume="0.35"
        loop="true" />

</scene>
```

The Player may retrieve the resource when required.

External resource access is subject to:

- network availability;
- URL accessibility;
- resource permissions;
- CORS or equivalent platform restrictions;
- Player security policies;
- resource availability.

The OVML Standard does not require a Player to access arbitrary URLs.

A Player MAY restrict external resources to trusted or explicitly allowed origins.

## 6. Asset Identifiers

A project MAY use logical asset identifiers instead of direct URLs.

Example:

```xml
<video src="forest-video" />
```

The identifier may be resolved through the project's asset catalog.

This allows the project to separate:

```text
logical asset identity
        ↓
actual resource location
```

The actual resource may later be:

- a remote URL;
- a local file;
- an OVMZ resource;
- a cloud object;
- another supported storage location.

This allows the same OVML content to be packaged or relocated without changing every media element.

## 7. OVMZ Resources

An OVMZ package MAY contain local resources.

Example:

```text
project.ovmz/
├── content.ovml
├── project.json
├── resources/
│   ├── audio/
│   ├── video/
│   └── images/
├── presets/
└── tts/
```

The OVML document may reference a packaged resource:

```xml
<video src="resources/video/intro.mp4" />
```

or use a project-specific asset identifier:

```xml
<video src="intro-video" />
```

where the package's resource manifest resolves the identifier.

The exact packaging rules are defined by the OVMZ specification.

## 8. Common Media Attributes

The following attributes may be used by media elements where applicable.

Attribute	Type	Default	Description
src	string	required	Media resource reference
layer	enum	implementation-defined	Rendering layer
volume	float	1.0	Audio volume
duration	float	implementation-defined	Requested playback duration in seconds
startTime	float	0	Start time in seconds
startMode	enum	absolute	Timing mode
startDelay	float	0	Additional start delay in seconds
loop	boolean	false	Repeat the media
trimStart	float	0	Skip the specified amount from the beginning
processing	string	implementation-defined	Processing preset reference
gridRow	integer	implementation-defined	Grid row
gridCol	integer	implementation-defined	Grid column
gridRowSpan	integer	implementation-defined	Number of grid rows occupied
gridColSpan	integer	implementation-defined	Number of grid columns occupied
sizePercent	integer	implementation-defined	Relative visual size

Not every attribute applies equally to every media type.

### 9. layer

The layer attribute defines the intended visual layer of a media element.

Supported values:

background
foreground
overlay

Example:

```xml
<video
    src="landscape"
    layer="background" />
```

and:

```xml
<img
    src="character"
    layer="foreground" />
```

The layer expresses the intended visual composition.

The complete layer model, including layering semantics, ordering, and the overlay layer, is defined
in:

[`reference/layers.md`](layers.md)

### 10. volume

The volume attribute controls the requested playback volume of an audio-capable media element.

The normal range is:

0.0 — 1.0

where:

0.0 = silent
1.0 = full volume

Example:

```xml
<audio
    src="background-music"
    volume="0.35" />
```

For video:

```xml
<video
    src="video-with-audio"
    volume="0.75" />
```

For media without an audio track, volume has no audible effect.

The final audio level may also be affected by:

Player volume;
system volume;
mixer settings;
audio processing;
user controls.
### 11. duration

The duration attribute specifies the requested amount of time for which the media element remains
active.

Example:

```xml
<video
    src="forest"
    duration="10" />
```

This requests a ten-second presentation.

duration does not necessarily mean that the underlying media file itself is ten seconds long.

For example:

```xml
<video
    src="long-video"
    duration="5" />
```

may use only five seconds of a longer source.

The exact behavior depends on the media type and available source data.

### 12. startMode

Media elements use the same timing model as other timed content.

Supported values are:

Value	Description
afterPrevious	Starts after the preceding sequential content
duringCurrent	Starts at a specified time within the current timing context
absolute	Starts at an absolute timeline position

For media elements, the default is:

absolute

Example:

```xml
<video
    src="intro"
    startMode="absolute"
    startTime="0" />
```

### 13. startTime

startTime specifies the requested start position in seconds.

Example:

```xml
<audio
    src="music"
    startMode="absolute"
    startTime="15.5" />
```

The media begins at the requested position on the applicable timeline.

The exact synchronization behavior is determined by the Player.

### 14. startDelay

startDelay adds an additional delay before media activation.

Example:

```xml
<audio
    src="sound-effect"
    startMode="absolute"
    startTime="10"
    startDelay="0.5" />
```

The requested activation point is therefore offset by the specified delay.

The value is expressed in seconds.

### 15. loop

The loop attribute requests repeated playback.

Default:

false

Example:

```xml
<audio
    src="ambient-forest"
    loop="true" />
```

A looping resource may be used for:

- background ambience;
- music;
- environmental sounds;
- repeating animation.

The Player determines how loop boundaries are handled.

A Player MAY apply gapless looping when supported by the underlying media system.

### 16. trimStart

The trimStart attribute specifies how much of the source media should be skipped before playback.

The value is expressed in seconds.

Example:

```xml
<video
    src="long-video"
    trimStart="12.5" />
```

The first 12.5 seconds of the source are omitted from playback.

trimStart does not modify the underlying asset.

It only changes how the asset is used by this media element.

### 17. processing

The processing attribute identifies a processing preset.

Example:

```xml
<video
    src="forest"
    processing="cinematic-dark" />
```

The identifier may refer to a preset defined by:

- the project;
- OpenVML Studio;
- an OVMZ package;
- a compatible plugin;
- another implementation-specific processing system.

For portable packages, processing presets MAY be included in:

presets/

The preset itself is separate from the media asset.

## 18. Grid Placement

Media elements MAY be positioned within a visual grid.

Example:

```xml
<video
    src="background"
    gridRow="1"
    gridCol="1"
    gridRowSpan="6"
    gridColSpan="6" />
```

The grid attributes are:

Attribute	Description
gridRow	Starting row
gridCol	Starting column
gridRowSpan	Number of rows occupied
gridColSpan	Number of columns occupied

The actual grid dimensions are determined by the consuming application or project configuration.

### 19. sizePercent

sizePercent defines the requested relative visual size of a visual media element.

Example:

```xml
<img
    src="character"
    sizePercent="50" />
```

A value of:

100

represents the full available reference size.

A value of:

50

requests approximately half of that size.

The exact interpretation depends on the layout system.

### 20. `<video>`

The `<video>` element represents video content.

Example:

```xml
<video
    src="intro"
    layer="background"
    volume="1"
    duration="10"
    startMode="absolute"
    startTime="0"
    startDelay="0"
    loop="false"
    trimStart="0"
    gridRow="1"
    gridCol="1"
    gridRowSpan="6"
    gridColSpan="6"
    sizePercent="100" />
```

A video may contain both visual and audio content.

The Player determines:

video decoding;
audio decoding;
synchronization;
frame presentation;
buffering;
hardware acceleration.
### 21. `<img>`

The `<img>` element represents still image content.

Example:

```xml
<img
    src="forest-background"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="8"
    sizePercent="100" />
```

An image has no intrinsic playback duration in the same sense as video.

When duration is specified, it defines how long the image element remains active in the project
timeline.

An image MAY therefore act as a visual background for a timed sequence.

### 22. `<audio>`

The `<audio>` element represents audio content.

Example:

```xml
<audio
    src="background-music"
    layer="background"
    volume="0.35"
    startMode="absolute"
    startTime="0"
    loop="true" />
```

Audio elements may be used for:

- music;
- ambience;
- sound effects;
- narration;
- pre-rendered speech;
- other supported audio content.

Audio does not require a visual representation.

## 23. Audio and TTS

Generated speech is logically an audio resource.

For a plain .ovml project, a `<line>` may reference a character whose voice is resolved at runtime.

The resulting speech may then be treated as an audio stream or generated audio resource by the
Player.

For example:

```xml
<line
    char="narrator"
    emotion="calm">
    Welcome to the lecture.
</line>
```

The OVML script does not require the generated TTS audio to be embedded directly into the document.

This allows the same script to work with:

- local TTS;
- remote TTS;
- system speech;
- pre-rendered speech;
- packaged speech assets.

## 24. Media Duration and Timeline

A media element may have a duration independent of the duration of its source.

For example:

```xml
<video
    src="source-video"
    trimStart="10"
    duration="5" />
```

Conceptually:

```text
Source
0s ─────── 10s ───────────── 15s ───────────>

              │<── 5 seconds ──>│
              playback
```

The source itself is not modified.

The media element defines a particular presentation of that source.

## 25. Reusing Assets

The same asset may be used multiple times.

Example:

```xml
<scene>

    <img
        src="castle"
        startMode="absolute"
        startTime="0"
        duration="10"
        sizePercent="100" />

    <img
        src="castle"
        startMode="absolute"
        startTime="30"
        duration="5"
        sizePercent="60" />

</scene>
```

The underlying asset is referenced twice.

Each media element has independent:

- timing;
- duration;
- size;
- position;
- processing;
- presentation properties.

## 26. Media and Scenes

Media elements normally occur within a scene.

Example:

```xml
<scene
    color="#1a1a2e"
    atmosphere="warm sunset">

    <video
        src="sunset"
        layer="background" />

    <audio
        src="birds"
        volume="0.4"
        loop="true" />

    <p>
        <line char="narrator">
            The sun was setting.
        </line>
    </p>

</scene>
```

The scene provides semantic context.

The media elements provide concrete media usage within that context.

## 27. Media and Camera

A scene may combine media elements with camera instructions.

Example:

```xml
<scene atmosphere="night city">

    <camera
        type="wide"
        target="city" />

    <video
        src="city-background"
        layer="background" />

    <img
        src="character"
        layer="foreground"
        sizePercent="60" />

</scene>
```

The camera describes the intended viewpoint.

The media elements describe the resources available to that viewpoint.

The Player determines how these instructions are composed by the rendering system.

## 28. Media Synchronization

Multiple media elements MAY share the same timeline position.

Example:

```xml
<video
    src="background"
    startMode="absolute"
    startTime="0" />

<audio
    src="music"
    startMode="absolute"
    startTime="0" />

<line
    char="narrator"
    startMode="absolute"
    startTime="0">
    The story begins.
</line>
```

The intended relationship is:

```text
0s
│
├── video
├── music
└── narration
```

The Player is responsible for maintaining synchronization during playback.

## 29. Resource Resolution

A Player resolving a media element generally performs the following conceptual process:

```text
Media Element
      │
      ▼
Resolve src
      │
      ├── external URL
      ├── asset ID
      ├── package resource
      └── implementation-defined source
      │
      ▼
Acquire Resource
      │
      ▼
Decode / Prepare
      │
      ▼
Apply Processing
      │
      ▼
Schedule
      │
      ▼
Render / Play
```

This process is intentionally implementation-independent.

Different Players may use different mechanisms while preserving the same OVML semantics.

## 30. Missing or Unavailable Resources

Failure to retrieve or decode a resource is a runtime condition.

For example:

```xml
<img src="https://example.com/missing.jpg" />
```

may be structurally valid OVML even if the resource is unavailable at playback time.

Similarly:

```xml
<audio src="unknown-asset" />
```

may be syntactically valid while failing during resource resolution.

A validator SHOULD validate the structure and reference format.

A Player MUST handle runtime resource failures according to its error-handling policy.

The OVML Standard does not require a universal placeholder image, silent audio track, or fallback
media.

## 31. Security

Players SHOULD treat external media references as untrusted resources.

A Player MAY restrict:

- allowed URL schemes;
- allowed domains;
- redirects;
- local filesystem access;
- cross-origin requests;
- embedded content;
- resource sizes.

These restrictions are runtime security policies and do not change the semantics of the OVML media
elements.

## 32. Validation

A validator SHOULD verify:

- src is present;
- numeric attributes contain valid numbers;
- boolean attributes contain valid boolean values;
- layer contains an allowed value;
- startMode contains an allowed value;
- volume is within the supported range;
- grid attributes contain valid integers;
- sizePercent contains a valid number;
- trimStart is not negative;
- duration is not negative.

The validator does not need to verify that an external URL is currently reachable.

It also does not determine whether:

- a media codec is supported;
- a remote server is available;
- a resource requires authentication;
- a processing preset exists at runtime;
- a Player can decode the resource.

## 33. Design Principle

OVML separates resources from their use in the timeline.

The same asset may therefore be reused with different timing and presentation instructions.

For example:

```text
Asset
    │
    ├── Media Element A
    │      ├── start = 0s
    │      ├── size = 100%
    │      └── layer = background
    │
    └── Media Element B
           ├── start = 30s
           ├── size = 50%
           └── layer = foreground
```

This distinction is essential for portability between plain OVML documents, OVMZ packages, and
rendered OVMV projects.

An asset is a resource. A media element is an instruction for using that resource within the OVML
timeline.

The OVML document describes the intended composition.

The Player determines how that composition is acquired, decoded, processed, synchronized, and
rendered on the target platform.
