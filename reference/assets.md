# Assets and Resource References

> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

**OVML Standard 2.2**

## 1. Purpose

Assets are external or packaged resources used by an OVML project.

An asset may contain:

- audio;
- video;
- images;
- music;
- sound effects;
- pre-rendered speech;
- other media resources supported by the Player.

OVML separates the **resource itself** from the **instruction that uses the resource**.

For example:

``xml`
<img src="forest" />

forest identifies a resource.

The <img> element describes how that resource is used in the current position of the script.

This separation allows the same asset to be reused with different timing, positioning, processing, and presentation settings.

2. Asset vs. Asset Reference

An asset is the actual resource.

An asset reference identifies that resource from an OVML document.

Conceptually:

Asset
  │
  │ identified by
  ▼
Asset Reference
  │
  ▼
Media Element

For example:

<video src="forest-video" />

The video file is the asset.

forest-video is the asset reference.

<video> is the instruction to use that asset in the project.

3. Asset Reference Forms

An OVML document MAY identify an asset using:

an external URL;
a logical asset ID;
a package-local path;
another resource identifier defined by a compatible implementation.

Examples:

<img src="https://cdn.example.com/images/forest.jpg" />
<img src="forest-background" />
<img src="resources/images/forest.jpg" />

The reference form does not change the semantic role of the asset.

4. External Assets

A plain .ovml document MAY reference assets hosted outside the document.

Example:

<scene atmosphere="quiet forest">

    <img
        src="https://cdn.example.com/images/forest.jpg"
        layer="background" />

    <audio
        src="https://cdn.example.com/audio/forest.mp3"
        volume="0.35"
        loop="true" />

</scene>

This allows an OVML document to remain small while referencing remotely hosted resources.

The Player is responsible for resolving and acquiring the resource.

External resource access depends on:

network availability;
resource availability;
Player security policy;
allowed domains;
platform restrictions;
access permissions.

A valid OVML document does not require every referenced resource to be embedded locally.

5. Allowed External Resources

Players MAY restrict which external resources can be loaded.

For security and portability, an implementation MAY define an allowlist of:

URL schemes;
domains;
content types;
resource sizes;
redirect targets.

For example, a Player may allow:

https://cdn.example.com/
https://assets.example.org/

while refusing unknown or unsafe origins.

The security policy belongs to the Player and does not change the OVML resource model.

6. Logical Asset IDs

A project SHOULD prefer logical asset identifiers when the project has an asset catalog.

Example:

<video src="intro-video" />

The identifier:

intro-video

does not necessarily expose the physical location of the resource.

The project may resolve it to:

https://cdn.example.com/video/intro.mp4

or:

resources/video/intro.mp4

or another supported resource.

This separation allows the resource location to change without changing the OVML script.

7. Asset Catalog

A project MAY maintain an asset catalog describing available resources.

A catalog can associate a logical ID with information such as:

id
type
location
mime type
duration
size
metadata

Example:

{
  "id": "forest-video",
  "type": "video",
  "location": "resources/video/forest.mp4"
}

The exact catalog format is outside the core OVML XML syntax unless defined by a specific package format.

8. Asset Types

Common asset types include:

Type	Examples
audio	music, ambience, sound effects
video	clips, backgrounds, animations
image	illustrations, photographs, backgrounds
speech	pre-rendered narration or dialogue
subtitle	subtitle or caption resources

Implementations MAY support additional resource types.

9. Audio Assets

Audio assets may represent:

background music;
ambience;
sound effects;
narration;
dialogue;
pre-rendered TTS;
other audio content.

Example:

<audio
    src="forest-ambience"
    volume="0.35"
    loop="true" />

The same audio asset may also be used by another element:

<audio
    src="forest-ambience"
    volume="0.15"
    startTime="30" />

The asset remains unchanged.

The two media elements specify different uses of it.

10. Video Assets

Video assets may contain:

recorded video;
animation;
visual backgrounds;
screen recordings;
rendered sequences;
other supported video content.

Example:

<video
    src="city-background"
    layer="background"
    duration="20" />

A video asset may contain its own audio track.

The Player determines whether that track is enabled and how it is synchronized with other audio content.

11. Image Assets

Image assets may contain:

photographs;
illustrations;
artwork;
backgrounds;
character images;
UI-related visual resources.

Example:

<img
    src="character-alex"
    layer="foreground"
    sizePercent="60" />

An image has no intrinsic playback duration.

Its duration is determined by the media element and the surrounding timeline.

12. Pre-rendered Speech Assets

Speech may be represented by an audio asset.

For example:

<audio src="tts/chapter01-line004.wav" />

This is particularly useful for packaged projects.

An OVMZ package MAY contain pre-rendered speech under:

tts/

The script may reference the generated audio through a project asset ID.

13. TTS References

OVML also supports projects in which speech is generated by the Player rather than packaged as an audio file.

For example:

<line char="alex">
    Welcome to the project.
</line>

The character definition may identify a voice.

The Player may then generate the speech at runtime.

This makes plain .ovml documents suitable for dynamic voice generation.

The actual TTS implementation may be:

a built-in Player voice;
a local TTS engine;
a system speech engine;
a remote TTS provider;
a plugin-provided provider;
a pre-rendered speech resource.

The OVML document describes the intended speech content.

The Player determines how that speech is produced.

14. Free and Built-in Voices

A Player MAY provide voices that can be used without user-supplied API credentials.

Such voices may include:

local voices;
system voices;
bundled voices;
free remote voice services;
other voices provided directly by the Player.

An OVML document MAY reference such a voice through the character definition.

The absence of an API key does not prevent an OVML document from being a valid document.

Voice availability is a runtime capability of the Player.

15. Asset Reuse

An asset MAY be referenced multiple times.

Example:

<scene>

    <img
        src="castle"
        startTime="0"
        duration="10"
        sizePercent="100" />

    <img
        src="castle"
        startTime="30"
        duration="5"
        sizePercent="60" />

</scene>

Both elements reference the same asset.

Each use may specify independent:

timing;
duration;
layer;
size;
position;
processing;
volume where applicable.
16. Asset Independence

Changing the physical location of an asset SHOULD NOT require changing the OVML script when a logical asset ID is used.

For example:

<video src="intro-video" />

may resolve to:

Development
    ↓
https://dev.example.com/intro.mp4

and later:

Production
    ↓
https://cdn.example.com/intro.mp4

The OVML document remains unchanged.

This provides portability between development, publishing, and packaged environments.

17. OVMZ Assets

An OVMZ package may contain all resources required for local playback.

Example:

project.ovmz/
├── content.ovml
├── project.json
├── resources/
│   ├── audio/
│   ├── video/
│   └── images/
├── presets/
└── tts/

The package may therefore contain:

images;
videos;
music;
sound effects;
pre-rendered speech;
processing presets.

The Player can resolve package-local resources without requiring access to their original external locations.

18. Plain OVML

A plain .ovml document does not need to contain all assets locally.

It may contain:

OVML document
      │
      ├── external media URLs
      ├── logical asset IDs
      └── runtime voice references

For example:

<ovml version="2.2" lang="en">

    <meta>
        <title>Forest Story</title>
    </meta>

    <cast>
        <character
            id="narrator"
            name="Narrator"
            voiceLang="en-US" />
    </cast>

    <assets>
        ...
    </assets>

    <script>

        <chapter>

            <scene>

                <img
                    src="https://cdn.example.com/forest.jpg"
                    layer="background" />

                <audio
                    src="https://cdn.example.com/forest.mp3"
                    volume="0.3"
                    loop="true" />

                <p>
                    <line char="narrator">
                        The forest was silent.
                    </line>
                </p>

            </scene>

        </chapter>

    </script>

</ovml>

The Player resolves the resources and may synthesize the narration at runtime.

19. OVMZ and Asset Materialization

When an OVML project is packaged as OVMZ, external or generated resources MAY be materialized into the package.

Conceptually:

Plain OVML
    │
    ├── external assets
    └── runtime TTS
          │
          ▼
       Export
          │
          ▼
OVMZ
    │
    ├── content.ovml
    ├── resources/
    └── tts/

This allows the resulting OVMZ project to be substantially more self-contained.

The original logical asset references MAY remain unchanged.

20. OVMV

OVMV represents a rendered audiovisual result.

In an OVMV workflow, the resources have already been processed into the final video representation.

Conceptually:

OVML
  │
  ├── assets
  ├── TTS
  ├── timing
  ├── scenes
  └── camera
        │
        ▼
     Rendering
        │
        ▼
      OVMV

The OVMV playback path does not need to resolve and reconstruct every original asset in order to present the rendered video.

21. Asset Resolution

A Player may conceptually resolve an asset using the following process:

Asset Reference
      │
      ▼
Identify reference type
      │
      ├── External URL
      │
      ├── Logical Asset ID
      │
      └── Package-local path
      │
      ▼
Resolve Resource
      │
      ▼
Validate Resource
      │
      ▼
Acquire / Open
      │
      ▼
Decode / Prepare
      │
      ▼
Present

The implementation may optimize or combine these stages.

The OVML Standard defines the semantic relationship, not the internal Player pipeline.

22. Missing Assets

A missing or unavailable asset is a runtime condition.

For example:

<img src="missing-background" />

may be structurally valid even if the corresponding resource cannot be found.

The Player SHOULD provide a controlled failure behavior.

Possible behaviors include:

skipping the resource;
displaying an implementation-defined placeholder;
reporting an error;
continuing playback;
stopping playback when the resource is essential.

The OVML Standard does not mandate a universal placeholder.

23. Asset Validation

A validator SHOULD verify the correctness of asset references according to the available project context.

It MAY verify:

reference syntax;
required attributes;
resource type compatibility;
logical ID format;
package-relative path format.

For external URLs, a validator is not required to verify that the resource is currently reachable.

Resource availability is a runtime concern.

24. Asset Security

Players MUST treat external resources as untrusted input.

Implementations SHOULD protect against:

unsupported URL schemes;
unauthorized local file access;
malicious redirects;
oversized resources;
unexpected content types;
resource exhaustion;
unsafe embedded content.

A Player MAY restrict resource access to explicitly allowed origins.

These restrictions are implementation-level security policies.

25. Portability

A portable OVML project SHOULD avoid unnecessary dependence on implementation-specific physical locations.

Logical references are preferred:

<video src="intro-video" />

over hard-coded local filesystem paths such as:

<video src="/home/user/project/assets/intro.mp4" />

Logical asset references allow the same project to move between:

Studio
   ↓
Plain OVML
   ↓
Web Player
   ↓
Desktop Player
   ↓
Mobile Player
   ↓
OVMZ

without changing the semantic content.

26. Asset and Processing Presets

An asset and its processing preset are separate concepts.

For example:

<img
    src="portrait"
    processing="warm-cinematic" />

Conceptually:

portrait
    │
    └── asset

warm-cinematic
    │
    └── processing preset

The same asset may therefore be presented using different processing presets.

This separation also allows presets to be packaged independently in OVMZ.

27. Asset and Scene Context

A scene may provide semantic information about how assets should be interpreted.

For example:

<scene
    color="#1a1a2e"
    atmosphere="warm sunset, quiet and peaceful">

    <audio
        src="ambient-forest" />

    <img
        src="forest-background"
        layer="background" />

</scene>

The atmosphere attribute may provide semantic context for tooling or AI-assisted asset selection.

The asset itself remains independent of the scene.

28. Asset Identity

An asset identity SHOULD remain stable within the scope in which it is referenced.

For example:

<img src="character-alex" />

should consistently identify the same logical resource within the project.

If the underlying physical resource changes, the logical asset ID may remain unchanged.

This allows projects to update assets without rewriting their scripts.

29. Design Principle

OVML deliberately separates:

Content
   ↓
Asset Reference
   ↓
Resource

and:

Resource
   ↓
Media Element
   ↓
Timeline
   ↓
Presentation

This makes it possible for the same semantic project to exist as:

Plain OVML
    ↓
external / logical resources
    ↓
runtime synthesis

        or

OVMZ
    ↓
bundled resources
    ↓
pre-rendered TTS

        or

OVMV
    ↓
fully rendered audiovisual result

The resource location is therefore not part of the creative meaning of the project.

An OVML asset is a resource. An asset reference identifies that resource. A media element defines how the resource participates in the project timeline.

This separation is fundamental to the portability of the Open Voice Markup Language.