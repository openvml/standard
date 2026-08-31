> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# Scene — `<scene>`

**OVML Standard 2.2**

## 1. Purpose

The `<scene>` element is the primary directorial context of an OVML script.

A scene groups content that belongs to the same dramatic, visual, narrative, or informational
context.

A scene may contain:

- `<camera>` directions;
- text blocks and character dialogue;
- images;
- video;
- audio;
- pauses;
- other script elements.

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

OVML separates the structure of a project from its final rendering.

A scene describes what belongs together from the author's or director's point of view.

It does not prescribe:

- which rendering engine must be used;
- which video codec must be used;
- which audio backend must be used;
- how media is buffered;
- how TTS is generated;
- how the camera is physically implemented.

Those decisions belong to the Player, renderer, or export pipeline.

## 3. Structure

A scene is represented by the `<scene>` element.

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
├── Camera
├── Location
├── Characters
├── Blocking
├── Prompt
├── Text (p / line)
├── Image
├── Video
├── Audio
└── Break
```

The exact set of allowed child elements is defined by the OVML specification and schema.

## 4. Scene Attributes

OVML 2.2 defines the following standard scene attributes.

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

## 5. color

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

The scene color is primarily semantic metadata.

It may be used by an OVML-compatible application for:

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

## 6. atmosphere

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
- the AI Assistant;
- asset-selection systems;
- preset-selection systems;
- future intelligent authoring tools.

One important purpose of atmosphere is AI-assisted asset selection.

For example:

```xml
<scene
    color="#1a1a2e"
    atmosphere="warm sunset, silence, tranquility">
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

The standard does not prescribe which AI model, asset catalogue, or recommendation algorithm is
used.

## 7. Additional Scene Attributes

Authoring environments and implementations MAY support additional scene attributes.

| Attribute | Type | Description |
| --------- | ---- | ----------- |
| id | string | Unique scene identifier |
| title | string | Display title of the scene |
| time | enum | Time of day: morning, afternoon, evening, night |
| weather | enum | Weather: clear, cloudy, rainy, foggy, snowy |
| mood | enum | Mood: calm, tense, mysterious, romantic, dramatic |
| duration | number | Maximum scene duration in seconds |
| transition | enum | Transition to the next scene: fade, cut, dissolve, wipe |

Example:

```xml
<scene
    id="ch02_forest"
    title="The Forest"
    time="morning"
    weather="foggy"
    mood="mysterious"
    color="#1a1a2e">

    ...
</scene>
```

These attributes carry useful structural metadata.

The standard requires no particular subset of them for a scene to be valid.

## 8. Scene Boundaries

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

## 9. Scene and Timeline

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

[`reference/timing.md`](timing.md)

## 10. Child Elements

A scene may contain the following child elements.

Element	Purpose
<location ref="...">	Reference to a canonical location from `<world>`
`<characters>`	List of characters present in the scene
`<blocking>`	Semantic character positioning and relationships
`<prompt>`	Prompt for AI-assisted image/video generation
`<camera>`	Camera or viewpoint direction within the scene
`<p>` / `<line>`	Text and dialogue blocks
`<img>` / `<video>` / `<audio>`	Media content
`<break>`	Explicit pause

## 11. Location Reference

A scene references a canonical location from the project's world canon through a child `<location>`
element with a ref attribute.

```xml
<scene
    color="#1a1a2e"
    atmosphere="tense, night">

    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>
```

```xml
    ...
</scene>
```

The ref value is the id of a location declared in the `<locations>` section of `<world>`.

Scene-specific change is expressed by a `<variation>` inside the scene, never by editing the canon:

```xml
<location ref="rusty_anchor">
    <variation>
        <time>night</time>
        <weather>foggy</weather>
        <changes>Chairs stacked, a fire in the hearth</changes>
    </variation>
</location>
```

A plain-text `<location>` without ref remains valid as a free-form description.

The complete location model is defined in:

[`reference/locations.md`](locations.md)

## 12. Characters

The `<characters>` element lists the characters participating in a scene.

Example:

```xml
<characters>
    <char ref="hero" emotion="thoughtful" />
    <char ref="heroine" emotion="curious" />
</characters>
```

Each `<char>` entry references a character id declared in the `<cast>` element.

The emotional state may provide additional context for voice direction and AI-assisted rendering.

1. [`reference/cast.md`](cast.md)

## 13. Blocking

The `<blocking>` element describes the semantic relationships between the characters present in the
scene.

Example:

```xml
<blocking>
    <character ref="anna" position="left" look_at="ivan" enters="true" />
    <character ref="ivan" position="right" addresses="anna" />
</blocking>
```

Blocking records intend and spatial relations rather than absolute coordinates:

- who is where;
- who looks at whom;
- who addresses whom;
- who reacts to whom;
- who enters or exits the scene.

The complete blocking model is defined in:

[`reference/blocking.md`](blocking.md)

## 14. Prompt

The `<prompt>` child of a scene provides a prompt for AI-assisted image or video generation.

```xml
<prompt>Dark mixed forest, morning fog, two people walking</prompt>
```

The prompt is optional.

It may be authored explicitly, or it may be auto-generated from the `<blocking>` element by an
AI-assisted workflow.

OVML does not prescribe which AI model or generator interprets the prompt.

## 15. Camera

A scene may contain one or more `<camera>` elements.

The `<camera>` element provides a directorial instruction within a scene.

Example:

```xml
<scene>

    <camera
        shot="medium"
        framing="center"
        target="alex"
        movement="static" />
```

```xml
    ...
</scene>
```

A camera is not a media file.

It describes how the visual content is intended to be presented.

The initial camera model supports:

- shot size;
- framing;
- camera movement;
- camera target;
- transition;
- duration;
- timing.

This is important for maintaining the separation between the standard and its implementation:

```text
OVML
   │
   │ camera instruction
   ▼
Player / Renderer
   │
   │ implementation
   ▼
visual result
```

The camera describes the intended viewpoint.

The media elements describe the resources available to that viewpoint.

The Player determines how these instructions are composed by the rendering system.

The full camera model is defined in:

[`concepts/camera.md`](../concepts/camera.md)

## 16. Text and Media Content

A scene may contain text blocks and media elements as direct children.

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

    <p>
        <line char="narrator">
            Welcome to the city.
        </line>
    </p>

</scene>
```

This allows a single scene to describe a complete audiovisual composition.

Text semantics are defined in:

[`reference/line.md`](line.md)
[`reference/paragraph.md`](paragraph.md)

Media semantics are defined in:

[`reference/media.md`](media.md)
[`reference/assets.md`](assets.md)

## 17. Scene Transitions

A transition between scenes may be represented by scene content or by future transition elements.

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

Explicit transition hints may be provided through scene metadata such as the transition attribute
described in section 7.

## 18. Scene Does Not Imply a Physical Location

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

## 19. Scene Does Not Imply a Fixed Duration

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

The scene may use this information to suggest or select appropriate presets.

However, the scene does not directly execute a preset unless an appropriate processing element or
character configuration explicitly specifies one.

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

The Assistant may use this information without changing the underlying OVML semantics.

## 22. Scene and Project Forms

Scenes are intentionally generic so that the same structure can support different types of OVML
projects.

#### Lecture

A scene may represent a section of a lesson.

```text
Lecture
 └── Chapter
      └── Scene
           ├── narration
           ├── slide
           └── media
```

#### Presentation

A scene may represent a presentation segment or slide composition.

#### Audiobook

A scene may represent a location, event, or dramatic moment.

#### Game Voiceover

A scene may represent a dialogue situation or game state.

#### Film Voiceover

A scene may represent a film sequence requiring synchronized dialogue and media.

#### Anime

A scene may contain dialogue, music, sound effects, visual media, and camera direction.

#### Podcast

A scene may represent a conversational segment.

The same `<scene>` element therefore serves multiple project forms.

## 23. Minimal Scene

The smallest useful scene can be:

```xml
<scene>
    <line char="narrator">
        Hello.
    </line>
</scene>
```

No additional metadata is required.

## 24. Fully Described Scene

A more expressive scene can combine semantic metadata, media, dialogue, and camera direction:

```xml
<scene
    color="#1a1a2e"
    atmosphere="warm sunset, silence, tranquility">

    <camera
        shot="wide"
        framing="center"
        movement="static" />

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

## 25. Validation

The OVML parser and validator are responsible for validating the structural correctness of a scene.

For example, they may verify:

- the `<scene>` element is properly opened and closed;
- attributes have valid names;
- color has a valid HEX format;
- required XML structure is respected;
- child elements are allowed in the given context.

The validator does not determine whether the scene is artistically appropriate.

For example, it should not attempt to determine whether a sad scene should really be blue.

That is a creative decision.

## 26. Director's Responsibility

OVML intentionally separates technical correctness from directing.

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

## 27. Summary

The `<scene>` element is a fundamental structural unit of OVML.

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

This makes `<scene>` suitable for every OVML project form, from a simple lecture or presentation to a
multi-character audiobook, interactive game narrative, film dubbing project, or animated production.

## 28. Related Documents

1. [`reference/document.md`](document.md)
2. [`reference/chapter.md`](chapter.md)
3. [`reference/body.md`](body.md)
4. [`reference/locations.md`](locations.md)
5. [`reference/blocking.md`](blocking.md)
6. [`reference/cast.md`](cast.md)
7. [`reference/line.md`](line.md)
8. [`reference/paragraph.md`](paragraph.md)
9. [`reference/media.md`](media.md)
10. [`reference/timing.md`](timing.md)
11. [`concepts/camera.md`](../concepts/camera.md)
12. [`concepts/scenes-and-world.md`](../concepts/scenes-and-world.md)
