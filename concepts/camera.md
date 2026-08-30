> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

OpenVML Camera Model

OpenVML 2.2

1. Overview

The <camera> element describes visual direction within an OpenVML scene.

It allows an author, director, or AI-assisted authoring system to describe how the visual composition should be framed and how the viewer's attention should move through a scene.

The camera model is intentionally declarative.

OVML describes the intended camera behavior.

The Player or rendering engine determines how that behavior is implemented on the target platform.

This distinction allows the same OpenVML project to be used by:

interactive Players;
video renderers;
presentation engines;
animation systems;
future 3D environments;
AI-assisted production tools.
2. Camera as Director's Instruction

A camera is not a media resource.

It is a directorial instruction.

For example:

<camera
`` `shot="close-up"
framing="center"
movement="static" />
`` `
does not mean that OpenVML must use a particular camera API.

It means:

Show the current visual composition as a centered close-up without camera movement.

The Player or renderer is responsible for realizing that instruction.

3. Relationship Between Scene and Camera

A camera belongs to the visual context of a scene.

Conceptually:

Project
│
└── Scene
    │
    ├── Camera
    │
    ├── Media
    │
    ├── Dialogue
    │
    └── Audio

The scene provides the context.

The camera provides visual direction.

Media provides the visual material.

Dialogue and audio provide narrative and auditory content.

4. Why Camera Is Part of OVML

OpenVML is not limited to describing a collection of media files.

A project may contain the same source assets but produce completely different results depending on visual direction.

For example:

Asset:
    character.png

could be shown as:

wide shot

or:

medium shot

or:

close-up

or:

extreme close-up

The <camera> element allows that directorial intention to be represented in the OpenVML document.

5. Initial Camera Model

OpenVML 2.2 introduces a deliberately compact camera model.

The first version focuses on the most useful concepts:

shot size;
framing;
camera movement;
camera target;
transition.

The model is intentionally extensible.

Future OpenVML versions may add more advanced camera capabilities without changing the basic concept.

6. Basic Camera

The simplest camera instruction is:

<camera
`` `shot="medium"
framing="center"
movement="static" />
`` `
This describes:

a medium shot;
centered composition;
no camera movement.
7. shot

The shot attribute describes the approximate size of the subject within the frame.

OpenVML 2.2 defines the following values:

Value	Description
extreme-wide	Very wide view establishing the environment
wide	Wide view showing the subject and surrounding environment
full	Full view of the subject
medium	Medium framing, typically showing the upper body or equivalent area
medium-close	Tighter framing between medium and close-up
close	Close-up emphasizing the subject
extreme-close	Very tight framing focused on a specific detail

Example:

<camera shot="close" />

The exact visual result depends on the available media and rendering environment.

8. framing

The framing attribute describes the position of the visual subject within the frame.

OpenVML 2.2 defines:

Value	Description
center	Subject centered in the frame
left	Subject positioned toward the left
right	Subject positioned toward the right
top	Subject positioned toward the upper part of the frame
bottom	Subject positioned toward the lower part of the frame

Example:

<camera
`` `shot="medium"
framing="left" />
`` `
This may be used to create visual space on the right side of the frame for another character, subtitles, graphics, or other content.

9. movement

The movement attribute describes the intended camera movement.

OpenVML 2.2 defines:

Value	Description
static	No camera movement
pan-left	Horizontal movement toward the left
pan-right	Horizontal movement toward the right
tilt-up	Vertical movement toward the top
tilt-down	Vertical movement toward the bottom
zoom-in	Gradual transition toward a closer framing
zoom-out	Gradual transition toward a wider framing
dolly-in	Movement toward the subject
dolly-out	Movement away from the subject

Example:

<camera
`` `shot="wide"
movement="zoom-in" />
`` `
The distinction between zoom and dolly is intentional.

A zoom changes the apparent framing through the camera's optical or rendering parameters.

A dolly represents physical movement of the camera relative to the subject.

A 2D renderer may approximate both using scaling and transformation.

10. target

The target attribute identifies the intended subject of the camera.

Example:

<camera
`` `shot="close"
framing="center"
target="alex" />
`` `
The target may refer to:

a character ID;
a visual object;
an element ID;
another addressable object defined by the implementation.

The exact target namespace depends on the project structure.

For a character, the value should correspond to the character's id in <cast>.

Example:

<character id="alex" name="Alex" />

followed by:

<camera
`` `shot="close"
target="alex" />
`` `11. Camera Transitions

A camera change may require a transition.

The transition attribute describes how the new camera state should replace the previous state.

OpenVML 2.2 defines:

Value	Description
cut	Immediate change
fade	Gradual fade between states
dissolve	Gradual visual blend
smooth	Smooth interpolation of camera parameters

Example:

<camera
`` `shot="close"
target="alex"
transition="smooth" />
`` `
If no transition is specified, the implementation may use an appropriate default.

For deterministic production workflows, authors should specify the transition explicitly when it matters.

12. Camera Duration

Camera instructions may have a duration when they represent a temporal camera state.

Example:

<camera
`` `shot="wide"
movement="zoom-in"
duration="5" />
`` `
This describes a five-second camera movement.

The temporal semantics follow the general OpenVML timing model.

The camera therefore participates in the project timeline in the same conceptual way as other timed elements.

See:

concepts/timeline-and-blocks.md
13. Camera Start Time

A camera instruction may use the standard timing attributes.

Example:

<camera
`` `shot="close"
target="alex"
startMode="absolute"
startTime="12"
duration="4" />
`` `
This requests:

12s ─────────────── 16s
     close-up
     Alex

The camera state becomes active at 12 seconds and remains active for four seconds.

14. Camera Sequences

A scene may contain multiple camera instructions.

Example:

<scene>

`` `<camera
    shot="wide"
    framing="center"
    movement="static"
    startMode="absolute"
    startTime="0"
    duration="5" />

<camera
    shot="medium"
    target="alex"
    movement="smooth"
    startMode="absolute"
    startTime="5"
    duration="4" />

<camera
    shot="close"
    target="alex"
    movement="zoom-in"
    startMode="absolute"
    startTime="9"
    duration="3" />
`` `
</scene>

Conceptually:

0s────────5s────────9s────────12s
│         │         │          │
│  wide   │ medium  │  close   │
│         │  Alex   │   Alex   │

This provides a basic shot sequence.

15. Camera and Scene Atmosphere

Camera direction may complement the semantic information of a scene.

Example:

<scene
`` `color="#1a1a2e"
atmosphere="ночь, тревога, опасность">

<camera
    shot="wide"
    movement="static" />

...
`` `</scene>

An AI Assistant may interpret the combination as:

Scene:
    night
    anxiety
    danger

Camera:
    wide
    static

and suggest alternative shots.

For example:

wide → medium → close

The Assistant may propose these changes, but the resulting OVML remains under the author's control.

16. Camera and Characters

Characters are natural camera targets.

Example:

<cast>
`` `<character id="alex" name="Alex" />
<character id="maria" name="Maria" />
`` `</cast>

<scene>

`` `<camera
    shot="medium"
    target="alex" />

<line char="alex">
    We have to leave.
</line>

<camera
    shot="medium"
    target="maria" />

<line char="maria">
    Now?
</line>
`` `
</scene>

This provides a simple dialogue shot structure.

The same concept can be used for:

film dubbing;
anime;
game dialogue;
animated stories;
narrated scenes.
17. Camera and Media

Camera instructions do not replace media.

For example:

<camera shot="close" target="alex" />

<img src="alex.png" />

The image remains the media resource.

The camera describes how that resource should be presented.

This distinction is important because the same asset may be used with different camera instructions.

18. Camera and 2D Media

OpenVML is not limited to 3D scenes.

A camera may operate on:

photographs;
illustrations;
generated images;
slides;
video;
2D animation;
composited media.

For a 2D renderer, camera operations may be implemented through:

cropping;
scaling;
translation;
interpolation;
viewport transformation.

The OVML document does not prescribe which technique must be used.

19. Camera and Video

For video content, camera instructions may describe how the video should be presented.

For example:

<camera
`` `shot="wide"
framing="center" />
`` `
<video src="scene.mp4" />

A Player or renderer may interpret this as a viewport or crop operation.

If the source video already contains a fixed camera perspective, the renderer may only be able to approximate the requested framing.

An implementation should not claim to provide a camera effect that the underlying media cannot support.

20. Camera and Image Assets

Camera instructions are particularly useful with large images.

For example:

<camera
`` `shot="wide"
movement="zoom-in"
duration="8" />
`` `
<img
`` `src="landscape"
duration="8" />
`` `
This can describe a classic documentary-style image movement:

wide
  ↓
gradual zoom
  ↓
closer framing

The actual implementation may use image scaling and viewport transformation rather than a physical camera.

21. Camera and AI-Generated Media

Camera instructions can also serve as production instructions for AI-generated visual content.

For example:

<scene atmosphere="dramatic confrontation">

`` `<camera
    shot="medium-close"
    framing="center"
    movement="dolly-in"
    target="alex" />

...
`` `</scene>

An AI-assisted workflow may use this information when generating or selecting visual assets.

The camera instruction therefore remains useful even when the final media is generated after the OVML script has been authored.

22. Camera Does Not Generate Media

The <camera> element does not itself create:

images;
video;
3D models;
animation;
visual effects.

It only describes visual direction.

For example:

<camera shot="close" target="alex" />

does not create a close-up image of Alex.

It tells a compatible renderer how the available visual content should be presented.

23. Camera and Rendering

The relationship can be represented as:

OVML
 │
 ├── Scene
 │    ├── Camera ────────┐
 │    ├── Image ─────────┤
 │    └── Video ─────────┤
 │                       ▼
 │                    Renderer
 │                       │
 │                       ▼
 │                  Final frame

The camera is therefore part of the description layer, not the rendering layer.

24. Camera and Playback

During interactive playback, the Player may resolve camera instructions dynamically.

For example:

Playback position
       │
       ▼
Current scene
       │
       ▼
Active camera
       │
       ▼
Current visual composition

When the user seeks through the project, the Player resolves the camera state corresponding to the new timeline position.

Seeking does not modify the OVML document.

25. Camera and Export

When an OVML project is exported to OVMV, camera instructions may be evaluated during video rendering.

Conceptually:

OVML
 │
 ├── timeline
 ├── scenes
 ├── media
 └── camera directions
          │
          ▼
     Video renderer
          │
          ▼
        OVMV

The resulting OVMV contains the rendered visual result rather than the original camera instructions as executable directives.

The original OVML document may still be preserved as project metadata or source material.

26. Camera and OVMZ

An OVMZ container may preserve camera instructions as part of the OVML source.

For example:

project.ovmz
│
├── content.ovml
│
├── resources/
│   ├── images/
│   ├── video/
│   └── audio/
│
└── presets/

The camera remains part of content.ovml.

This allows the project to remain editable and renderable on another compatible system.

27. Camera Is Declarative

The camera model follows the central OpenVML principle:

The author describes the desired visual result. The implementation determines how to achieve it.

An implementation may use:

HTML/CSS transforms;
WebGL;
WebGPU;
native rendering;
FFmpeg;
GPU video pipelines;
2D canvas;
3D engines;
other technologies.

The OVML document remains independent of these technologies.

28. Camera Limitations

Not every Player or renderer will support every camera feature equally.

For example:

a simple audio-focused Player may ignore camera instructions;
a 2D renderer may approximate a dolly movement;
a video-only renderer may support only crop and scale;
an interactive 3D renderer may implement full camera movement.

An implementation should preserve the semantics it supports and clearly define unsupported capabilities.

Camera instructions must not make otherwise valid OVML documents syntactically invalid merely because a particular Player cannot fully render them.

29. Validation

The parser and validator are responsible for technical validity.

They may validate:

proper opening and closing of <camera>;
supported attributes;
valid enumeration values;
numeric timing values;
valid references where required;
valid XML structure.

The validator does not judge the artistic quality of the camera direction.

For example:

<camera
`` `shot="extreme-close"
movement="zoom-out"
duration="0.5" />
`` `
may be an unusual creative decision, but it is not necessarily invalid.

30. Future Extensibility

The initial camera model is intentionally small.

Future OpenVML versions may introduce:

explicit camera position;
focal length;
field of view;
depth of field;
focus target;
camera paths;
rotation;
3D coordinates;
easing functions;
subject tracking;
multiple simultaneous cameras;
advanced transitions.

Such features should extend the declarative camera model rather than couple OVML to a specific rendering engine.

31. Summary

The <camera> element provides a machine-readable representation of visual direction.

It allows OpenVML to describe:

shot size;
framing;
subject;
camera movement;
camera transitions;
camera timing.

The camera model works together with <scene>:

Scene
│
├── atmosphere ── semantic context
├── color ─────── visual context
│
├── camera ────── visual direction
│
├── media ─────── visual material
├── line ──────── narrative/dialogue
└── audio ─────── auditory material

The fundamental principle is:

<scene> describes the context. <camera> describes the visual direction. Media provides the visual material. The Player or renderer determines how the director's intent is realized.

This makes the camera model suitable for everything from simple image presentations to audiobooks with visual scenes, animated stories, anime, game narratives, film dubbing, and full video production workflows.