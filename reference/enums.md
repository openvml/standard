> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# Enumeration Values

**OVML Standard 2.2**

## 1. Purpose

This document consolidates the enumeration values used by OVML attributes.

An enumeration attribute accepts one of a fixed set of values.

This reference provides a single authoritative list of those values per attribute.

A conforming validator SHOULD verify that enumeration attributes contain allowed values.

## 2. Time of Day

The `time` attribute on a scene or scene variation expresses the time of day.

Allowed values:

Value	Description
morning	Morning
afternoon	Afternoon
evening	Evening
night	Night

Example:

```xml
<scene id="ch1_tavern_night" time="night" mood="tense">
    ...
</scene>
```

The time may also be expressed in a `<variation>`:

```xml
<variation>
    <time>night</time>
</variation>
```

## 3. Weather

The `weather` attribute on a scene or scene variation expresses the weather.

Allowed values:

Value	Description
clear	Clear weather
cloudy	Cloudy
rainy	Rainy
foggy	Foggy
snowy	Snowy

Example:

```xml
<variation>
    <weather>rainy</weather>
</variation>
```

## 4. Mood

The `mood` attribute on a scene expresses the scene's mood.

Allowed values:

Value	Description
calm	Calm
tense	Tense
mysterious	Mysterious
romantic	Romantic
dramatic	Dramatic

Example:

```xml
<scene id="ch1_office" time="evening" mood="dramatic">
    ...
</scene>
```

## 5. Emotion

The `emotion` attribute on a line or character reference expresses the speaker's emotional state.

Allowed values:

Value	Description
neutral	Neutral
happy	Happy
sad	Sad
angry	Angry
surprised	Surprised
fearful	Fearful
disgusted	Disgusted
excited	Excited
confused	Confused
contemptuous	Contemptuous
crying	Crying
laughing	Laughing
shouting	Shouting
whispering	Whispering / speaking softly
singing	Singing

Taken together with intonation, emotion forms the delivery vocabulary of a line.

1. [`reference/emotions.md`](emotions.md)

## 6. Intonation

The `intonation` attribute on a line expresses the delivery pattern.

Allowed values:

Value	Description
statement	Statement
question	Question
exclamation	Exclamation
command	Command
sarcasm	Sarcasm
irony	Irony
whisper	Whisper
shout	Shout

1. [`reference/emotions.md`](emotions.md)

## 7. Position

The `position` attribute on a blocking character expresses its intended position.

Allowed values:

Value	Description
left	Left
right	Right
center	Center
offscreen-left	Offscreen to the left
offscreen-right	Offscreen to the right

1. [`reference/blocking.md`](blocking.md)

## 8. Layer

The `layer` attribute on a media element expresses its intended compositing layer.

Allowed values:

Value	Description
background	Background layer
foreground	Foreground layer
overlay	Overlay layer

An implementation MAY support additional internal layers.

Example:

```xml
<img
    src="character"
    layer="foreground" />
```

1. [`reference/media.md`](media.md)

## 9. startMode

The `startMode` attribute expresses the timing mode of a content element.

Allowed values:

Value	Description
afterPrevious	Start after the preceding sequential content
duringCurrent	Start at a specified time relative to the current timing context
absolute	Start at an absolute timeline position

Example:

```xml
<video
    src="intro"
    startMode="absolute"
    startTime="0" />
```

The default for media elements is:

absolute

1. [`reference/timing.md`](timing.md)

## 10. Transition

The `transition` type and the scene `transition` attribute express how the presentation changes.

Allowed values:

Value	Description
fade	A gradual fade
cut	An immediate switch
dissolve	A crossfade
wipe	A directional wipe

Example:

```xml
<transition type="dissolve" duration="0.5" />
```

or on a scene:

```xml
<scene ... transition="fade" />
```

1. [`reference/transitions.md`](transitions.md)

## 11. Timbre

The `timbre` attribute expresses the acoustic character of a voice.

Enumeration values:

Value	Description
neutral	Neutral
breathy	Breathy, with aspiration
rough	Hoarse, coarse
soft	Soft, gentle
nasal	Nasal
belting	Theatrical
whispering	Whispering

Timbre may also be expressed as descriptive text on a character.

1. [`reference/voice.md`](voice.md)

## 12. Role

The `role` attribute on a character expresses its narrative role.

The enumeration values used across the standard include:

protagonist	Protagonist / main character
antagonist	Antagonist / primary opposing character
major	Major character
minor	Minor character
narrator	Narrator or narrative voice

The character reference vocabulary also includes:

main	Main character
villain	Villain or primary opposing character

The complete set of values accepted by a conforming implementation:

protagonist
antagonist
major
minor
narrator
main
villain

Example:

```xml
<character
    id="narrator"
    name="Narrator"
    role="narrator" />
```

1. [`reference/cast.md`](cast.md)

## 13. Gender

The `gender` attribute on a character expresses its gender classification.

Allowed values:

Value	Description
male	Male
female	Female
neutral	Neutral or unspecified

1. [`reference/cast.md`](cast.md)

## 14. Age

The `age` attribute on a character expresses its age category.

Enumeration values:

child	Child
teen	Teenager
young_adult	Young adult
adult	Adult
elderly	Elderly
senior	Senior / pensioner

Example:

```xml
<character
    id="vestfal"
    name="Vestfal"
    gender="male"
    age="adult" />
```

1. [`reference/cast.md`](cast.md)

## 15. Validation

An OVML validator SHOULD verify that enumeration attributes contain allowed values.

Enumeration values are structural: using an unknown value is a validity concern.

The interpretation of a valid value remains semantic and is handled by the Player.

## 16. Related Documents

1. [`reference/emotions.md`](emotions.md)
2. [`reference/voice.md`](voice.md)
3. [`reference/blocking.md`](blocking.md)
4. [`reference/media.md`](media.md)
5. [`reference/timing.md`](timing.md)
6. [`reference/transitions.md`](transitions.md)
7. [`reference/cast.md`](cast.md)
