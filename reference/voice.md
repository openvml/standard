> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Voice, Timbre, and Pronunciation

**OVML Standard 2.2**

## 1. Purpose

OVML provides voice and pronunciation attributes that describe how a character's speech is intended to sound.

These attributes may appear on a character or on an individual line.

They are declarative hints. The TTS engine or Player determines how they are interpreted.

## 2. Voice Parameters

The following voice parameters describe the speaker's delivery.

Attribute	Type	Range	Description
pitch	float	-10 to +10	Pitch (0 = normal)
rate	float	0.5 to 2.0	Speech rate
volume	float	0.0 to 2.0	Volume
timbre	enum	see below	Voice timbre

These values apply a requested delivery to the speaker.

## 3. pitch

The `pitch` attribute controls the base pitch of the voice.

For the raw voice-parameter model, the range is:

-10 to +10

where 0 represents the normal pitch.

A character may also use a pitch expressed as a multiplier.

For the character attribute model:

Attribute	Type	Default	Description
pitch	float	1.0	Base pitch multiplier

Recommended range:

0.5 — 2.0

The exact interpretation depends on the selected voice engine.

## 4. rate

The `rate` attribute controls the speed of speech.

Default:

1.0

Recommended range:

0.5 — 2.0

Example:

<character
```
id="vestfal"
name="Vestfal"
rate="0.9" />
```

This value represents the character's base speech rate.

A line MAY specify its own speech rate, which takes precedence for that particular line.

## 5. volume

The `volume` attribute controls the loudness of the voice.

Range:

0.0 — 2.0

A value of 1.0 represents normal volume.

## 6. Timbre

The `timbre` attribute describes the acoustic character of the voice.

The enumeration values are:

Value	Description
neutral	Neutral
breathy	Breathy, with aspiration
rough	Hoarse, coarse
soft	Soft, gentle
nasal	Nasal
belting	Theatrical
whispering	Whispering

Example line-level use:

<line
```
char="hero"
timbre="rough">
Where have you been?
```
</line>

## 7. Timbre as Descriptive Text

In the character model, timbre may also be represented as descriptive text rather than a fixed enumeration.

A character MAY contain a `<timbre>` element.

Example:

<timbre>
    Deep, warm and slightly rough male voice.
</timbre>

or:

<timbre>
    Soft female voice with a clear tone and restrained warmth.
</timbre>

The descriptive form allows the value to be used with different voice systems and AI-assisted workflows.

The OVML Standard does not require a particular TTS engine to interpret timbre.

## 8. Emotion

The `emotion` attribute describes the emotional state of the speaker.

Allowed values include:

neutral, happy, sad, angry, surprised, fearful, disgusted, excited, confused, contemptuous, crying, laughing, shouting, whispering, singing

Example:

<line char="hero" emotion="angry">
    Get out of here!
</line>

See: reference/emotions.md

## 9. Intonation

The `intonation` attribute describes the delivery pattern of a line.

Allowed values:

statement, question, exclamation, command, sarcasm, irony, whisper, shout

Example:

<line char="general" intonation="command">
    Hold the line!
</line>

See: reference/emotions.md

## 10. Pronunciation Control

For precise control of the pronunciation of individual words, OVML provides the `<w>` element.

The `<w>` element may be used to:

set explicit stress;
provide an alias for abbreviations;
supply a phonetic transcription;
ignore a word during TTS.

## 11. Explicit Stress

The `stress` attribute marks the stressed syllable.

Syntax:

stress="слог+слог"

The syllable marked with `+` receives the stress.

Example:

<p char="hero">
```
Он жил в <w stress="зáмок">замок</w> и думал о <w stress="зáмок">замок</w>.
```
</p>

## 12. Abbreviation Aliases

The `alias` attribute replaces a word during synthesis.

This is useful for abbreviations and initialisms.

Example:

<p char="narrator">
```
Организация <w alias="Эф Би Ай">FBI</w> провела расследование.
<w alias="США">USA</w> — великая страна.
```
</p>

## 13. Phonetic Transcription

The `ph` attribute provides a phonetic transcription, which may use IPA or the SAPI alphabet.

Example:

<p char="hero">
```
Сложное слово: <w ph="kʲɪˈtaj">китай</w>
```
</p>

## 14. Ignoring Words During TTS

An empty alias may be used to skip a word during TTS.

Example:

<p char="hero">
```
Кодекс: <w alias="">{code_123}</w>
```
</p>

## 15. Word Element Attributes

Attribute	Type	Description
stress	string	Stressed-syllable notation
alias	string	Replacement text used during synthesis
ph	string	Phonetic transcription (IPA or SAPI)

## 16. Line-Level Voice Attributes

A spoken line may override the character's default voice parameters for that line.

Supported line-level attributes include:

emotion;
intonation;
pitch;
rate;
volume;
timbre;
emphasis.

Example:

<line
```
char="anna"
emotion="excited"
pitch="1.2"
rate="1.1">
We won!
```
</line>

## 17. Narrative Voice

A narrator may be configured with a voice tone.

Example:

<narrator voice="deep">Могучий голос рассказчика.</narrator>
<narrator voice="soft">Тихий, интимный тон.</narrator>

The `voice` attribute on narration is a descriptive hint.

## 18. Validation vs. Runtime

An OVML validator SHOULD verify:

numeric voice parameters contain valid numeric values;

`timbre`, `emotion`, and `intonation` contain allowed values when defined as enumerations;

`<w>` elements contain valid attribute values.

The validator does not determine:

how a TTS engine renders a timbre;

whether an emotion is well performed;

whether a phonetic transcription is correct.

These are runtime, provider, or creative concerns.

## 19. Related Documents

See: reference/cast.md
See: reference/emotions.md
See: reference/line.md
See: reference/tts.md
