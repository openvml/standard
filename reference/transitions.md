> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Transitions

**OVML Standard 2.2**

## 1. Purpose

A transition describes how the presentation changes from one content element to another.

Transitions may occur:

between scenes;

between media blocks;

at a point in the timeline.

A transition defines how the visual change is presented, in contrast to timing, which defines when the change occurs.

## 2. Transition Values

OVML defines the following transition types.

Value	Description
fade	A gradual fade between content
cut	An immediate switch between content
dissolve	A crossfade during which content overlaps
wipe	A wipe that reveals the new content

These values are used by both the `<transition>` element and the `transition` attribute on a scene.

## 3. The Transition Element

A `<transition>` element may express a transition between two elements.

Example:

<transition type="fade" duration="1.0" from="scene_01" to="scene_02" />
<transition type="cut" />
<transition type="dissolve" duration="0.5" />
<transition type="wipe" direction="left" duration="0.3" />

## 4. Attributes of the Transition Element

Attribute	Type	Description
type	enum	The transition type: fade, cut, dissolve, wipe
duration	number	Transition duration in seconds
from	string	Source element of the transition
to	string	Destination element of the transition
direction	string	Direction hint for transitions such as wipe

The `type` attribute is the primary descriptor.

The other attributes refine how the transition is applied.

## 5. fade

A fade gradually transitions between content.

<transition type="fade" duration="1.0" from="scene_01" to="scene_02" />

The duration controls how long the fade takes.

## 6. cut

A cut switches immediately between content.

<transition type="cut" />

A cut has no meaningful duration.

## 7. dissolve

A dissolve crosses one element into another.

<transition type="dissolve" duration="0.5" />

During a dissolve, the outgoing and incoming content overlap.

## 8. wipe

A wipe reveals the new content with a directional wipe.

<transition type="wipe" direction="left" duration="0.3" />

The direction is a hint for how the wipe is performed.

## 9. Scene Transition Attribute

A `<scene>` may carry a `transition` attribute that describes the transition to the next scene.

Example:

<scene id="ch1_night" time="night" mood="tense" transition="fade">
    ...
</scene>

Allowed values:

fade
cut
dissolve
wipe

This is a declarative intent at the scene level.

## 10. Transitions vs. Scene Boundaries

A scene boundary does not necessarily imply a visible transition.

For example, two scenes may follow each other without a fade or cut.

The scene `transition` attribute makes the author's transition intent explicit.

A scene boundary without a transition leaves the transition behavior to the author or rendering extension.

## 11. Transitions and Timing

A transition is a time-based presentation event.

Timing determines when the transition occurs.

The transition determination defines how the visual change occurs.

Timing
  │
  └── WHEN

Transition
  │
  └── HOW THE CHANGE IS PRESENTED

Transitions may overlap other content depending on their semantics.

## 12. Breaks and Pauses

A pause (silence) is distinct from a transition.

A pause may be expressed with a `<break>` element.

<break duration="1.0" />
<break duration="0.5" description="короткая пауза" />
<break duration="3.0" description="длинная пауза" />

A break introduces silence in the sequential flow.

A transition changes how one piece of visual content becomes another.

See: reference/break.md

## 13. Validation

An OVML validator SHOULD verify:

the transition `type` contains an allowed value;

`duration`, when present, is a valid non-negative number;

the `transition` attribute on a scene contains an allowed value.

Transition rendering is a runtime concern of the Player.

## 14. Related Documents

See: reference/timing.md
See: concepts/scenes-and-world.md
See: reference/media.md
See: reference/enums.md
