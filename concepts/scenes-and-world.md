> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

OpenVML Scenes

OpenVML 2.2

## 1. Overview

A scene is a logical and creative unit of an OpenVML project.

A scene groups content that belongs to the same dramatic, visual, narrative, or informational
context.

A scene may contain:

- dialogue;
- narration;
- images;
- video;
- audio;
- pauses;
- camera instructions;
- other OpenVML elements.

A scene can represent, for example:

- a location in a story;
- a moment in an audiobook;
- a slide or part of a presentation;
- a shot sequence;
- a conversational exchange;
- a lesson segment;
- a visual composition;
- a transition between two parts of a project.

The scene provides context for its contents but does not itself perform rendering.

## 2. Scene as a Creative Unit

OpenVML separates the structure of a project from its final rendering.

A scene describes:

What belongs together from the author's or director's point of view.

It does not prescribe:

- which rendering engine must be used;
- which video codec must be used;
- which audio backend must be used;
- how media is buffered;
- how TTS is generated;
- how the camera is physically implemented.

Those decisions belong to the Player, renderer, or export pipeline.

## 3. Basic Structure

A scene is represented by the <scene> element.

Example:

```xml
<scene>
    <line char="narrator">
        The sun was setting over the city.
    </line>

    <img
        src="sunset"
        startMode="absolute"
        startTime="0"
        duration="10" />
</scene>
```

A scene may contain multiple blocks.

Conceptually:

```text
Scene
│
├── Line
├── Line
├── Audio
├── Image
├── Video
├── Break
└── Camera
```

The exact set of allowed child elements is defined by the corresponding element specifications and
the OVML schema.

## 4. Scene Boundaries

A scene establishes a boundary between two creative contexts.

For example:

```xml
<scene atmosphere="morning, quiet">
    ...
</scene>

<scene atmosphere="evening, crowded">
    ...
</scene>
```

The first scene may represent one location or emotional context, while the second represents
another.

Scene boundaries can therefore be used by authoring tools, Players, and AI-assisted workflows to
understand the structure of a project.

A scene boundary does not necessarily imply a visible transition.

For example, two scenes may follow each other without a fade or cut.

## 5. Scene Attributes

OpenVML 2.2 defines the following scene attributes.

Attribute	Type	Required	Description
color	HEX color	No	Color associated with the scene
atmosphere	string	No	Description of the scene's atmosphere or mood

Both attributes are optional.

A scene may therefore be completely valid without either attribute:

```xml
<scene>
    ...
</scene>
```

### 6. color

The color attribute specifies a color associated with the scene.

Example:

```xml
<scene color="#1a1a2e">
    ...
</scene>
```

The value uses the hexadecimal RGB format:

#RRGGBB

For example:

#000000
#ffffff
#1a1a2e
#ff6600

## 7. Purpose of color

The scene color is primarily semantic metadata.

It may be used by an OpenVML-compatible application for:

- scene identification in the UI;
- timeline visualization;
- scene markers;
- authoring hints;
- visual grouping;
- preset selection;
- AI-assisted workflows;
- rendering hints.

For example, an editor may display:

```text
┌─────────────────────────────────────────┐
│ Scene 03                                │
│ #1a1a2e                                 │
│                                         │
│ Evening — abandoned city                │
└─────────────────────────────────────────┘
```

The standard does not require a Player to reproduce the scene color directly in the final media.

## 8. Scene Color and Processing Presets

The scene color may be used as a hint when selecting visual processing presets.

For example:

```xml
<scene
    color="#1a1a2e"
    atmosphere="warm sunset, silence, calm">
```

An authoring system or AI Assistant may use the color as an additional visual cue when selecting:

- image processing presets;
- video processing presets;
- color grading;
- visual effects;
- backgrounds.

However, color does not directly execute a processing operation.

It is descriptive metadata.

### 9. atmosphere

The atmosphere attribute describes the atmosphere, mood, or emotional context of a scene.

Example:

```xml
<scene
    atmosphere="warm sunset, silence, tranquility">
    ...
</scene>
```

The value is a free-form string.

It is written in the language of the project.

For example:

```xml
<scene atmosphere="тёплый закат, тишина, умиротворение">
```

or:

```xml
<scene atmosphere="warm sunset, silence, tranquility">
```

Both are valid.

## 10. Atmosphere Is Semantic Information

atmosphere is not a rendering command.

It describes the author's intent.

For example:

```xml
<scene atmosphere="тревога, дождь, пустая улица">
```

does not mean that the Player must automatically:

- add rain;
- add a dark color filter;
- generate a sound effect;
- change the music.

Instead, it provides semantic information that can be used by:

- the author;
- OpenVML Studio;
- AI Assistant;
- asset-selection systems;
- preset-selection systems;
- future intelligent authoring tools.

## 11. AI-Assisted Asset Selection

One important purpose of atmosphere is AI-assisted asset selection.

For example:

```xml
<scene
    color="#1a1a2e"
    atmosphere="тёплый закат, тишина, умиротворение">
```

An AI Assistant may interpret this as a request for assets such as:

Images:
    sunset
    warm landscape
    evening sky

Audio:
    quiet ambience
    soft wind
    distant birds

Music:
    calm
    warm
    reflective

The Assistant may then suggest assets from the available asset catalogue.

The standard does not prescribe which AI model, asset catalogue, or recommendation algorithm is
used.

## 12. Scene Context

The scene provides context to the blocks contained within it.

For example:

```xml
<scene atmosphere="ночной город, дождь">

    <line char="narrator">
        The city never slept.
    </line>

    <audio src="rain" />

    <img src="city-night" />

</scene>
```

A human reader can understand that:

- the narration;
- rain;
- city image;

belong to the same creative context.

An AI Assistant can use the same information when helping the author modify the scene.

### 12.1 Scene and the World Canon

A scene may reference a canonical entity of its project via a
child element with a ref attribute.

The canonical entities are declared once in the project-level
<world> element, in free-form sections (locations, terms, and so on).

Example — a location reference:

```xml
<scene>
    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>

    <line char="narrator">
        The heavy door of the tavern closed behind her.
    </line>
</scene>
```

The ref value is the id of a canonical entity.

The mechanism is identical for every world section. A lecture scene may
reference a term the same way:

```xml
<term ref="photosynthesis" />
```

Scene-specific change is expressed by <variation>:

```xml
<location ref="rusty_anchor">
    <variation>
        <time>night</time>
        <changes>Broken bottle by the door</changes>
    </variation>
</location>
```

The world canon keeps canonical entities consistent across the many
scenes of a long project.

The complete model is defined in:

reference/world.md

## 13. Scene and Timeline

A scene participates in the project's timeline through the elements it contains.

The scene itself does not necessarily have a startTime or duration.

Instead, the temporal behavior is determined by its child blocks.

For example:

```xml
<scene>
    <line char="alex">
        Hello.
    </line>

    <video
        src="city"
        startMode="absolute"
        startTime="0"
        duration="15" />
</scene>
```

The video has an explicit temporal position.

The line may use sequential timing.

The scene provides their common structural context.

The general timing model is defined in:

concepts/timeline-and-blocks.md

## 14. Scene and Dialogue

Scenes are particularly useful for dialogue-driven content.

Example:

```xml
<scene atmosphere="quiet conversation">

    <line char="alex">
        Are you coming?
    </line>

    <line char="maria">
        In a minute.
    </line>

    <line char="alex">
        I'll wait.
    </line>

</scene>
```

The scene groups the dialogue while the timing model determines the sequence.

This is useful for:

- audiobooks;
- game dialogue;
- film dubbing;
- anime;
- interactive narratives;
- podcasts.

## 15. Scene and Media

A scene can combine different media types.

Example:

```xml
<scene color="#182033">

    <video
        src="background"
        layer="background"
        startMode="absolute"
        startTime="0"
        duration="20" />

    <audio
        src="ambience"
        startMode="absolute"
        startTime="0"
        duration="20"
        volume="0.5" />

    <line char="narrator">
        Welcome to the city.
    </line>

</scene>
```

This allows a single scene to describe a complete audiovisual composition.

## 16. Scene and Camera

A scene may contain camera-related instructions.

For example:

```xml
<scene atmosphere="dramatic">

    <camera>
        ...
    </camera>

    <video src="background" />

    <line char="narrator">
        Something was approaching.
    </line>

</scene>
```

The <camera> element describes visual direction.

Camera semantics are defined separately in:

concepts/camera.md

The important architectural distinction is:

A scene provides the context. The camera provides visual direction.

## 17. Scene Hierarchy

Scenes belong to the script structure of an OpenVML project.

Conceptually:

```text
OVML
│
└── script
    │
    ├── chapter
    │   │
    │   ├── scene
    │   │   ├── block
    │   │   ├── block
    │   │   └── block
    │   │
    │   └── scene
    │
    └── chapter
```

The exact chapter structure is defined by the main OVML specification.

Scenes should be treated as structural units rather than independent documents.

## 18. Scene Transitions

A transition between scenes may be represented by the content of the scenes or by future transition
elements.

A scene boundary by itself does not require:

- a fade;
- a cut;
- a crossfade;
- a sound effect;
- a camera transition.

For example:

```xml
<scene atmosphere="day">
    ...
</scene>

<scene atmosphere="night">
    ...
</scene>
```

does not prescribe how the Player transitions between them.

The author or future rendering extensions may provide explicit transition instructions.

## 19. Scene Color and Atmosphere Are Independent

color and atmosphere provide different kinds of information.

color

Describes a visual cue.

color="#1a1a2e"
atmosphere

Describes semantic and emotional context.

atmosphere="тёплый закат, тишина, умиротворение"

They can be used independently:

```xml
<scene color="#1a1a2e">
```

or:

```xml
<scene atmosphere="quiet evening">
```

or together:

```xml
<scene
    color="#1a1a2e"
    atmosphere="quiet evening, warm sunset">
```

## 20. Scene and Presets

Scenes can provide contextual information for processing presets.

For example, a processing system may use:

```text
Scene
 ├── color
 ├── atmosphere
 │
 └── media
      ├── image
      ├── video
      └── audio
```

The system may use this information to suggest appropriate presets.

However, the scene does not directly reference or execute a preset unless an appropriate processing
element or character configuration explicitly specifies one.

This distinction keeps the scene declarative.

## 21. Scene and the AI Assistant

The scene is an important semantic boundary for AI-assisted authoring.

An Assistant can treat a scene as a contextual unit when performing tasks such as:

- selecting assets;
- suggesting music;
- suggesting sound effects;
- selecting processing presets;
- proposing camera directions;
- generating dialogue;
- adjusting atmosphere;
- proposing transitions;
- checking continuity.

For example:

```xml
<scene
    color="#243447"
    atmosphere="ночь, дождь, напряжение">
```

provides substantially more context than an isolated media block.

The Assistant may use this information without changing the underlying OpenVML semantics.

## 22. Scene and Project Types

Scenes are intentionally generic so that the same structure can support different types of OpenVML
projects.

Lecture

A scene may represent a section of a lesson.

```text
Lecture
 └── Chapter
      └── Scene
           ├── narration
           ├── slide
           └── media
```

### Presentation

A scene may represent a presentation segment or slide composition.

Audiobook

A scene may represent a location, event, or dramatic moment.

Game Voiceover

A scene may represent a dialogue situation or game state.

Film Voiceover

A scene may represent a film sequence requiring synchronized dialogue and media.

Anime

A scene may contain dialogue, music, sound effects, visual media, and camera direction.

Podcast

A scene may represent a conversational segment.

The same <scene> element therefore serves multiple project forms.

## 23. Scene Does Not Imply a Physical Location

A scene does not necessarily mean a physical place.

For example:

Scene 01 — Introduction
Scene 02 — Main argument
Scene 03 — Conclusion

may be used in a presentation.

Likewise:

Scene 01 — Character introduction
Scene 02 — Conflict
Scene 03 — Resolution

may be used in an audiobook.

The meaning of a scene is determined by the project context.

## 24. Scene Does Not Imply a Fixed Duration

A scene does not require a duration attribute.

Its effective duration may be determined by the content it contains.

For example:

```text
Scene
 ├── dialogue       5s
 ├── music          20s
 └── video          15s
```

The resulting temporal extent depends on the resolved timeline.

This allows scenes to remain semantic structures rather than forcing authors to manually maintain
redundant duration metadata.

## 25. Validation

The OpenVML parser and validator are responsible for validating the structural correctness of a
scene.

For example, they may verify:

- the <scene> element is properly opened and closed;
- attributes have valid names;
- color has a valid format;
- required XML structure is respected;
- child elements are allowed in the given context.

The validator does not determine whether the scene is artistically appropriate.

For example, it should not attempt to determine whether:

"a sad scene should really be blue."

That is a creative decision.

## 26. Director's Responsibility

OpenVML intentionally separates technical correctness from directing.

The author or director decides:

- where a scene begins;
- where it ends;
- what happens inside it;
- what atmosphere it represents;
- what visual mood it should have;
- which characters participate;
- what media is used;
- how the scene should feel.

The standard provides a machine-readable language for expressing those decisions.

The Player executes the resulting description.

## 27. Minimal Scene

The smallest useful scene can be:

```xml
<scene>
    <line char="narrator">
        Hello.
    </line>
</scene>
```

No additional metadata is required.

## 28. Fully Described Scene

A more expressive scene can combine semantic metadata, media, dialogue, and camera direction:

```xml
<scene
    color="#1a1a2e"
    atmosphere="warm sunset, silence, tranquility">

    <camera>
        ...
    </camera>

    <video
        src="sunset"
        layer="background"
        startMode="absolute"
        startTime="0"
        duration="12" />

    <audio
        src="wind"
        startMode="absolute"
        startTime="0"
        duration="12"
        volume="0.3" />

    <line char="narrator">
        The sun slowly disappeared beyond the horizon.
    </line>

</scene>
```

This example demonstrates the intended relationship between:

```text
Scene
 ├── semantic context
 │    ├── color
 │    └── atmosphere
 │
 ├── visual direction
 │    └── camera
 │
 ├── visual media
 │    └── video
 │
 ├── audio environment
 │    └── audio
 │
 └── narrative content
      └── line
```

## 29. Summary

The <scene> element is a fundamental structural unit of OpenVML.

It provides:

- logical grouping;
- narrative context;
- visual context;
- emotional context;
- a boundary for AI-assisted authoring;
- a context for media and dialogue;
- a context for camera direction.

The two standard scene attributes are:

color
atmosphere

color provides a visual semantic hint.

atmosphere provides a descriptive semantic hint.

Neither attribute directly commands the Player to perform a particular rendering operation.

The fundamental principle is:

A scene describes the director's intent and context. It does not dictate the implementation used to
realize that intent.

This makes <scene> suitable for every OpenVML project form, from a simple lecture or presentation to
a multi-character audiobook, interactive game narrative, film dubbing project, or animated
production.
