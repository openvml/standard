# Timing Model

> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, spatial composition, and synchronization.

**OVML Standard 2.2**

## 1. Purpose

The OVML timing model defines **when content becomes active, how long it remains active, and how
different content elements are synchronized**.

OVML is not limited to a linear sequence of events.

A scene may contain multiple independent content streams that overlap in time:

- background audio;
- background video;
- background images;
- foreground audio;
- foreground video;
- foreground images;
- TTS and dialogue;
- subtitles and text;
- camera events;
- transitions.

These elements may start and end independently.

For example:

```text
TIME       0────1────2────3────4────5────6────7────8

BG image   █████████████████████████████████████████

BG audio   █████████████████████████████████████████

TTS        ███████████████████████████████████

FG image             ███████████████████
                     3──────────────8

FG video                    ███████████
                            4───────7
```

The foreground image does not wait for the TTS to finish.

The foreground video may appear in the middle of the TTS.

The background media may continue throughout the entire scene.

This overlapping composition is a fundamental part of OVML.

## 2. Core Principle

OVML describes temporal intent.

The document specifies:

- what content should be active;
- when it should become active;
- how long it should remain active;
- how it relates to other content;
- where visual content should appear.

The Player determines how that intent is implemented.

Therefore:

OVML defines when content should occur. The Player determines how that content is buffered,
streamed, synthesized, decoded, rendered, and synchronized on the target platform.

The standard does not prescribe:

- audio buffer implementation;
- video decoding;
- network buffering;
- TTS implementation;
- threading;
- hardware acceleration;
- rendering technology;
- synchronization algorithms.

## 3. The OVML Composition Model

An OVML scene can be understood as a composition of independently scheduled elements.

                         OVML COMPOSITION

```text
                              TIMELINE
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
     BACKGROUND              FOREGROUND              TEXT / TTS
          │                       │                       │
     ┌────┼────┐             ┌────┼────┐                  │
     │    │    │             │    │    │                  │
   Audio Video Image       Audio Video Image              Line
     │    │    │             │    │    │                  │
     └────┴────┘             └────┴────┘                  │
          │                       │                       │
          └───────────────────────┼───────────────────────┘
                                  │
                           SYNCHRONIZATION
                                  │
                               PLAYER
```

This model separates several independent dimensions.

### Time

When does the element exist?

### Layer

What is its compositing layer?

### Spatial position

Where does a visual element appear?

### Duration

How long does the element remain active?

### Content

What is actually played or displayed?

These dimensions should not be confused.

## 4. Content Elements

The timing model applies to different types of content.


| Element | Temporal | Visual | Audio | Layer |
| :--- | :--- | :--- | :--- | :--- |
| `<line>` | yes | yes | yes | text/content |
| `<img>` | yes | yes | no | background / foreground |
| `<video>` | yes | yes | optionally | background / foreground |
| `<audio>` | yes | no | yes | background / foreground |
| `<break>` | yes | no | no | none |
| `<camera>` | yes | indirectly | no | camera |
| `transitions` | yes | yes | optionally | presentation |


Not every element uses every timing or spatial property.

## 5. Independent Timing

A content element MAY have its own start time and duration.

For example:

```xml
<img
    src="background"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="20" />

<line
    char="narrator"
    startMode="absolute"
    startTime="2">
    The story begins.
</line>

<img
    src="character"
    layer="foreground"
    startMode="absolute"
    startTime="5"
    duration="4"
    gridRow="2"
    gridCol="4" />
```

Conceptually:

```text
TIME       0────1────2────3────4────5────6────7────8────9────10

BG image   ███████████████████████████████████████████████████

TTS              ███████████████████████████████████

FG image                       ███████████████████
                               5──────────────9
```

The elements are independent.

## 6. Overlapping Content

OVML explicitly supports overlapping content.

Elements MUST NOT be assumed to block one another unless their timing relationship explicitly
requires sequential execution.

For example:

```xml
<line char="narrator">
    The city disappeared behind us.
</line>

<video
    src="city"
    layer="foreground"
    startMode="duringCurrent"
    startTime="2"
    duration="5"
    gridRow="2"
    gridCol="3" />
```

The video may begin while the TTS line is still playing.

The Player does not need to wait for the line to finish.

## 7. Background Content

Background content provides persistent or supporting content behind other visual elements.

Typical background elements include:

- background images;
- background video;
- ambient audio;
- background music.

Example:

```xml
<img
    src="forest"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="30" />

<audio
    src="forest-ambience"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="30"
    volume="0.25" />
```

The background elements may remain active while dialogue and foreground content change.

## 8. Foreground Content

Foreground content appears above background visual content.

Typical foreground elements include:

- character images;
- character videos;
- visual effects;
- overlays;
- foreground audio;
- temporary visual inserts.

Example:

```xml
<img
    src="character-alex"
    layer="foreground"
    startMode="duringCurrent"
    startTime="3"
    duration="5"
    gridRow="2"
    gridCol="4"
    gridRowSpan="2"
    gridColSpan="2" />
```

The element may appear at any point during another active element.

## 9. Background and Foreground Are Independent of Timing

layer describes compositing semantics.

startMode, startTime, and duration describe temporal semantics.

These are independent dimensions.

For example:

```xml
<video
    src="background"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="20" />

<video
    src="character"
    layer="foreground"
    startMode="absolute"
    startTime="7"
    duration="4" />
```

The first element is background because of layer.

The second is foreground because of layer.

Their timing is determined separately.

## 10. Images Have Temporal Lifetime

An image is static media, but it may still have a temporal lifetime.

Example:

```xml
<img
    src="castle"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="15" />
```

This means:

Display the image beginning at 0 seconds and keep it active for 15 seconds.

The image does not need an intrinsic playback duration.

Its lifetime is defined by the OVML document.

## 11. Audio Has Temporal Lifetime

Audio may also have an explicit presentation duration.

Example:

```xml
<audio
    src="rain"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="60"
    loop="true" />
```

The Player may loop the source to satisfy the requested presentation duration.

Audio timing is independent from visual grid positioning.

## 12. Video Has Temporal Lifetime

Video can define:

- start position;
- duration;
- source trim;
- looping;
- layer;
- spatial position.

Example:

```xml
<video
    src="explosion"
    layer="foreground"
    startMode="absolute"
    startTime="12.5"
    duration="3"
    trimStart="1.2"
    gridRow="2"
    gridCol="3"
    gridRowSpan="2"
    gridColSpan="2" />
```

This defines both temporal and spatial behavior.

## 13. TTS Is a Time-Based Content Stream

A `<line>` rendered through TTS is not necessarily a fixed-duration object.

Its actual duration may depend on:

- text;
- voice;
- TTS provider;
- TTS engine;
- pitch;
- rate;
- generated audio;
- Player implementation.

For example:

```xml
<line char="alex">
    This sentence is synthesized at runtime.
</line>
```

The actual playback duration may only become known after synthesis.

Therefore, sequential content may depend on the actual runtime duration of TTS.

## 14. TTS and Overlapping Foreground Content

Foreground media MAY begin while TTS is playing.

Example:

```xml
<line
    char="narrator"
    startMode="absolute"
    startTime="0">
    The door opened slowly and something appeared in the darkness.
</line>

<video
    src="door"
    layer="foreground"
    startMode="absolute"
    startTime="3"
    duration="5"
    gridRow="2"
    gridCol="3"
    gridRowSpan="2"
    gridColSpan="2" />
```

Conceptually:

```text
TIME       0────1────2────3────4────5────6────7────8

TTS        █████████████████████████████████

FG video             ███████████████████
                     3──────────────8
```

The video is synchronized to the timeline, not forced to wait for TTS completion.

### 15. startMode

OVML defines three primary timing modes:

Mode	Meaning
afterPrevious	Start after the preceding sequential content
duringCurrent	Start at a specified time relative to the current timing context
absolute	Start at a specified position on the timeline

These modes allow both sequential and overlapping composition.

### 16. afterPrevious

afterPrevious creates a sequential relationship.

Example:

```xml
<line char="narrator" startMode="afterPrevious">
    First sentence.
</line>

<line char="alex" startMode="afterPrevious">
    Second sentence.
</line>
```

Conceptually:

```text
First sentence
██████████████
              │
              ▼
              Second sentence
              █████████████
```

The second element begins after the preceding sequential element has completed.

For dynamically generated TTS, the actual completion time may only be known at runtime.

### 17. duringCurrent

duringCurrent allows an element to begin while another element is active.

Example:

```xml
<line char="narrator">
    The spaceship rose slowly into the dark sky.
</line>

<video
    src="spaceship"
    layer="foreground"
    startMode="duringCurrent"
    startTime="3"
    duration="5"
    gridRow="2"
    gridCol="3" />
```

The video begins at the specified point within the current timing context.

This is particularly useful for:

visual inserts;
sound effects;
character appearances;
animations;
camera events;
synchronized media.
### 18. absolute

absolute places an element at a defined timeline position.

Example:

```xml
<audio
    src="music"
    layer="background"
    startMode="absolute"
    startTime="0"
    loop="true" />

<video
    src="logo"
    layer="foreground"
    startMode="absolute"
    startTime="8"
    duration="3" />
```

The video begins at 8 seconds regardless of the duration of preceding TTS.

Absolute timing is therefore suitable for precise synchronization.

### 19. startTime

startTime is expressed in seconds.

Fractional values are allowed.

Example:

```xml
<video
    src="effect"
    startMode="absolute"
    startTime="12.5" />
```

The element begins at 12.5 seconds.

When used with duringCurrent, the value represents a time relative to the applicable timing context.

When used with absolute, it represents a position on the project timeline.

### 20. startDelay

startDelay adds a delay to an element's scheduled start.

Example:

```xml
<line
    char="narrator"
    startMode="afterPrevious"
    startDelay="1">
    The next sentence begins one second later.
</line>
```

startDelay belongs to the element being delayed.

It is different from `<break>`.

### 21. break

A `<break>` introduces an explicit pause into sequential script flow.

Example:

```xml
<line char="narrator">
    Everything became silent.
</line>

<break time="1500" />

<line char="narrator">
    Then I heard a sound.
</line>
```

A break affects sequential flow.

It does not globally pause independent background or foreground media.

For example:

```text
TTS        ████████████
BREAK                  ───
BG audio   █████████████████████████
BG video   █████████████████████████
```

The background elements may continue during the break.

See break.md.

## 22. Duration

duration defines how long an element remains active in the presentation.

Example:

```xml
<img
    src="character"
    layer="foreground"
    startTime="5"
    duration="4" />
```

The element is active from approximately:

```text
5s → 9s
```

Duration does not determine the source media's intrinsic length.

It determines the element's intended presentation lifetime.

## 23. Source Duration vs Presentation Duration

A media asset may have its own intrinsic duration.

OVML may define a different presentation duration.

For example:

```text
Source video:
0 ───────────────────────── 30s
```

```text
OVML presentation:
        5 ─────── 12s
```

The Player is responsible for applying the requested presentation timing.

The asset itself is not modified by the OVML timing declaration.

### 24. trimStart

trimStart changes the position from which source media begins.

Example:

```xml
<video
    src="movie"
    trimStart="10"
    startTime="5"
    duration="4" />
```

This means:

```text
Project timeline:
5s ───────────── 9s
```

```text
Source:
10s ──────────── 14s
```

startTime controls the project timeline.

trimStart controls the source timeline.

These values represent different dimensions.

## 25. Looping

Media may be looped.

Example:

```xml
<audio
    src="ambient"
    layer="background"
    startTime="0"
    duration="30"
    loop="true" />
```

The Player repeats the source as required to maintain the element for the requested duration.

Looping is especially useful for:

- ambient sounds;
- background music;
- atmospheric video;
- animated backgrounds.

## 26. Visual Position and Timing

Timing and spatial position are independent.

For example:

```xml
<video
    src="dragon"
    layer="foreground"
    startTime="4"
    duration="5"
    gridRow="2"
    gridCol="4"
    gridRowSpan="2"
    gridColSpan="2" />
```

The timing says:

```text
4s → 9s
```

The grid says:

row 2  
column 4  
row span 2  
column span 2  

Therefore:

Timing answers WHEN. Grid answers WHERE.

## 27. Foreground Elements May Appear in Any Grid Cell

A foreground element is not restricted to a fixed position.

Example:

```xml
<img
    src="character"
    layer="foreground"
    startTime="3"
    duration="4"
    gridRow="1"
    gridCol="1" />
```

Another element may appear later in another position:

```xml
<img
    src="character"
    layer="foreground"
    startTime="8"
    duration="4"
    gridRow="4"
    gridCol="5" />
```

The Player renders each element according to its declared spatial and temporal properties.

## 28. Multiple Foreground Elements

Multiple foreground elements may overlap.

Example:

```xml
<img
    src="character"
    layer="foreground"
    startTime="3"
    duration="6"
    gridRow="2"
    gridCol="2" />

<video
    src="effect"
    layer="foreground"
    startTime="5"
    duration="2"
    gridRow="3"
    gridCol="4" />
```

Conceptually:

```text
TIME       0────1────2────3────4────5────6────7────8────9

Character             █████████████████████████
                      3──────────────────9

Effect                            ███████
                                  5───7
```

The two elements may coexist.

## 29. Background and Foreground Audio

The background and foreground layer concepts may also be applied to audio.

Example:

```xml
<audio
    src="forest"
    layer="background"
    startTime="0"
    duration="30"
    loop="true"
    volume="0.25" />

<audio
    src="bird"
    layer="foreground"
    startTime="8"
    duration="2"
    volume="0.8" />
```

The background ambience continues while the foreground sound effect occurs.

The layer does not refer to screen position for audio.

It expresses its role in the composition.

## 30. Audio and Visual Synchronization

Audio and visual elements may be scheduled independently but synchronized through the common
timeline.

Example:

```xml
<audio
    src="explosion"
    startTime="5"
    duration="2" />

<video
    src="explosion-video"
    startTime="5"
    duration="2"
    layer="foreground" />
```

Both elements begin at the same timeline position.

The Player is responsible for minimizing playback drift between them.

## 31. Background Image + TTS + Foreground Video

A common OVML composition may look like:

```xml
<img
    src="space"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="20" />

<audio
    src="space-ambience"
    layer="background"
    startMode="absolute"
    startTime="0"
    duration="20"
    loop="true" />

<line
    char="narrator"
    startMode="absolute"
    startTime="1">
    We had been travelling for three days.
</line>

<video
    src="spaceship"
    layer="foreground"
    startMode="duringCurrent"
    startTime="4"
    duration="6"
    gridRow="2"
    gridCol="3"
    gridRowSpan="2"
    gridColSpan="2" />
```

Conceptually:

```text
TIME       0────1────2────3────4────5────6────7────8────9────10

BG image   ███████████████████████████████████████████████████

BG audio   ███████████████████████████████████████████████████

TTS           ███████████████████████████████████████

FG video                    ███████████████████████████
                            4──────────────────10
```

This is a fundamental OVML use case.

## 32. Sequential Does Not Mean Global

afterPrevious creates a sequential relationship only for the applicable content flow.

It does not mean:

Stop every other element until this element finishes.

For example:

```xml
<audio
    src="music"
    layer="background"
    startTime="0"
    loop="true" />

<line char="narrator">
    The story continues.
</line>

<video
    src="character"
    layer="foreground"
    startMode="duringCurrent"
    startTime="2"
    duration="4" />
```

The background music continues.

The TTS plays.

The foreground video starts during the TTS.

All three streams coexist.

## 33. Timing Context

Timing relationships are interpreted within the structure of the document.

Conceptually:

```text
Project
  │
  ├── Chapter
  │     │
  │     └── Scene
  │           │
  │           ├── Background
  │           ├── Foreground
  │           ├── TTS
  │           ├── Break
  │           └── Camera
  │
  └── Chapter
```

The document structure provides organization.

The timeline provides temporal relationships.

The two concepts should remain separate.

## 34. Scenes Do Not Automatically Reset Time

A `<scene>` is a semantic and visual grouping mechanism.

It does not automatically imply a global timeline reset.

For example:

```xml
<scene>
    <line char="narrator">
        Scene one.
    </line>
</scene>

<scene>
    <line char="narrator">
        Scene two.
    </line>
</scene>
```

The Player determines the sequential relationship according to the applicable document timing rules.

If an explicit pause is required, it should be represented explicitly.

## 35. Camera Timing

Camera instructions participate in the same temporal composition.

Example:

```xml
<camera
    startMode="absolute"
    startTime="5"
    duration="4" />
```

The camera event may therefore overlap:

- TTS;
- background video;
- foreground images;
- foreground video;
- transitions.

Camera behavior is defined separately in camera.md.

## 36. Transitions and Timing

Transitions are also time-based presentation events.

They may overlap other content depending on their semantics.

The timing model determines when the transition occurs.

The transition definition determines how the visual change occurs.

This distinction is important:

```text
Timing
  │
  └── WHEN
```

```text
Transition
  │
  └── HOW THE CHANGE IS PRESENTED
```

## 37. Seeking

The Player SHOULD be able to determine the active elements at a requested timeline position.

For example:

```text
TIME       0────5────10────15────20────25

BG image   ███████████████████████████████

BG audio   ███████████████████████████████

TTS              █████████

FG image                ███████

FG video                     █████████
```

If the user seeks to 17 seconds, the Player determines which elements should currently be active.

OVML does not prescribe the internal seeking algorithm.

## 38. Runtime Duration

Some durations are known from the document.

Others may only become known at runtime.

For example:

```xml
<line char="narrator">
    Dynamically synthesized speech.
</line>
```

The final audio duration depends on the TTS engine and generated audio.

Therefore, the Player may need to resolve sequential timing dynamically.

This is not a validation problem.

## 39. Validation vs Runtime

The validator checks whether the timing declarations are structurally valid.

For example:  

Is startTime a valid number?  
Is duration non-negative?  
Is startMode allowed?  
Is time valid for `<break>`?  

The Player handles runtime concerns:  
  
Can the asset be loaded?  
How long did TTS synthesis take?  
Can the media be decoded?  
How should overlapping streams be synchronized?  
How should the element be rendered?  
  
The validator does not predict runtime behavior.  

## 40. Timing Precision

Time values expressed in seconds MAY contain fractional values.

Examples:  

startTime="2.5"  
duration="0.75"  
startDelay="1.25"  

This allows sufficiently precise synchronization for:

- speech;
- sound effects;
- subtitles;
- animation;
- video;
- camera events.

The Player's internal clock resolution is implementation-defined.

## 41. Deterministic Composition

Given the same:

- OVML document;
- asset versions;
- voice configuration;
- processing configuration;
- Player implementation;

the Player SHOULD preserve the same declared temporal relationships.

Actual wall-clock behavior may vary because of:

- network latency;
- buffering;
- TTS generation time;
- decoding latency;
- hardware;
- platform scheduling.

These implementation details do not change the semantic timing model.

## 42. Timing Events

A Player MAY expose runtime timing events.

Examples include:  

PlaybackStarted  
ChapterChanged  
SceneChanged  
BlockChanged  
LineChanged  
MediaCue  
PlaybackCompleted  

These events are runtime concepts.

They do not become part of the OVML document.

They may be consumed by:

- the Player UI;
- subtitles;
- plugins;
- accessibility features;
- external integrations.

## 43. Design Principles

The OVML timing model follows several principles.

### 43.1 Elements are independently schedulable

A content element does not automatically block another element.

### 43.2 Overlap is normal

Simultaneous TTS, audio, video, and images are valid and expected.

### 43.3 Timing and spatial composition are separate

Timing answers:

When?

Grid answers:

Where?

Layer answers:

Above or below what?

### 43.4 Sequential timing is only one mode

afterPrevious is useful for dialogue and narration, but it does not define the entire OVML timing
model.

### 43.5 The Player owns execution

OVML describes intent.

The Player implements execution.

## 44. Summary

The OVML timing model can be summarized as:

```text
                         OVML

                  WHAT SHOULD EXIST
                         │
                         ▼
              ┌─────────────────────┐
              │ Content Elements    │
              ├─────────────────────┤
              │ Line / TTS          │
              │ Image               │
              │ Video               │
              │ Audio               │
              │ Break               │
              │ Camera              │
              └──────────┬──────────┘
                         │
                         ▼
                    WHEN
              ┌─────────────────────┐
              │ startMode           │
              │ startTime           │
              │ startDelay          │
              │ duration            │
              │ trimStart           │
              │ loop                │
              └──────────┬──────────┘
                         │
                         ▼
                    WHERE
              ┌─────────────────────┐
              │ layer               │
              │ gridRow             │
              │ gridCol             │
              │ row/col span        │
              └──────────┬──────────┘
                         │
                         ▼
                  PLAYER EXECUTION
              ┌─────────────────────┐
              │ TTS                 │
              │ Buffering           │
              │ Decoding            │
              │ Synchronization     │
              │ Rendering           │
              └─────────────────────┘
```

The fundamental rule is:

OVML describes a temporal and spatial composition of independent content elements.

And the central architectural principle is:

OVML defines when and where content should occur. The Player determines how that content is
buffered, streamed, synthesized, decoded, rendered, and synchronized on the target platform.
