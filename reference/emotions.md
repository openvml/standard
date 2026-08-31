> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# Emotions and Intonation

**OVML Standard 2.2**

## 1. Purpose

The `emotion` and `intonation` attributes describe how a character's voice should deliver a line.

`emotion` describes the emotional state of the speaker.

`intonation` describes the delivery pattern of the line.

Both are declarative voice hints. They do not prescribe a particular TTS engine, voice, or
audio-processing implementation.

## 2. Where Emotion Appears

The `emotion` attribute may appear on a `<line>` or other spoken content block, and on a character
reference within a `<characters>` element of a scene.

Example:

```xml
<line char="alex" emotion="surprised">
    You came after all!
</line>
```

A scene may describe the emotion of a participating character:

```xml
<scene id="ch1_tavern_night" time="night" mood="tense">
    <characters>
        <char ref="heroine" emotion="cautious"/>
    </characters>
</scene>
```

## 3. Emotion Values

The `emotion` attribute uses a fixed enumeration.

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

## 4. Emotion Is Descriptive

The `emotion` value is descriptive intent.

It does not require a particular TTS provider to implement it in a specific way.

The Player or TTS engine determines how an emotion is rendered, if at all.

A Player MAY ignore the emotion attribute during playback.

## 5. Emotion on Lines

The `emotion` attribute may appear on a spoken line.

```xml
<line
    char="anna"
    emotion="happy">
    What a wonderful day!
</line>
```

A line-level emotion applies to that particular line.

It may differ from a character's default voice configuration.

## 6. Intonation

The `intonation` attribute describes the delivery pattern of a line.

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

## 7. Intonation on Lines

The `intonation` attribute may appear on a spoken line.

```xml
<line
    char="heavy"
    intonation="command">
    Halt! Do not move.
</line>
```

Intonation describes how the line is delivered.

It is a hint to the TTS engine or Player.

## 8. Relationship Between Emotion and Intonation

`emotion` and `intonation` describe different aspects of delivery.

emotion

Describes the speaker's emotional state.

intonation

Describes the prosodic delivery pattern.

They may be combined:

```xml
<line
    char="alex"
    emotion="angry"
    intonation="command">
    Stop right now.
</line>
```

Both attributes are optional.

## 9. Emotion in Scenes

A scene may use an explicit `mood` attribute as well as per-character emotion.

Example:

```xml
<scene id="ch1_tavern_night" time="night" mood="tense">
    <characters>
        <char ref="heroine" emotion="cautious"/>
    </characters>
</scene>
```

The scene `mood` is a vocabulary value such as calm, tense, mysterious, romantic, or dramatic.

Per-character emotion describes an individual character's state.

1. [`reference/enums.md`](enums.md)

## 10. Validation

An OVML validator SHOULD verify:

`emotion`, when present, contains an allowed value from the enumeration;

`intonation`, when present, contains an allowed value from the enumeration.

Emotion and intonation are semantic hints.

The validator does not determine whether a particular rendering of an emotion is correct.

## 11. Related Documents

1. [`reference/cast.md`](cast.md)
2. [`reference/enums.md`](enums.md)
3. [`reference/line.md`](line.md)
4. [`reference/voice.md`](voice.md)
