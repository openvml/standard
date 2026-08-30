# Break Element

> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

**OVML Standard 2.2**

## 1. Purpose

The `<break>` element explicitly introduces a period of silence or inactivity into the script timeline.

It is primarily intended for:

- pauses in narration;
- pauses between dialogue lines;
- dramatic pauses;
- pauses between sections of content;
- controlling the rhythm of spoken content.

A break does not contain content and does not reference an asset.

Example:

`` `<break time="1000" />

This introduces a pause of 1000 milliseconds.

2. Syntax

The basic syntax is:

<break time="1000" />

The <break> element is self-closing.

It MUST NOT contain child elements.

3. Attributes
time

The time attribute specifies the duration of the pause.

Attribute	Type	Required	Description
time	integer	yes	Duration of the break in milliseconds

Example:

<break time="500" />

The example represents a 500 ms pause.

4. Time Unit

The value of time is expressed in milliseconds.

Examples:

<break time="250" />

250 milliseconds.

<break time="1000" />

1 second.

<break time="2500" />

2.5 seconds.

Using milliseconds provides sufficient precision for speech and audiovisual timing.

5. Valid Values

The value SHOULD be a non-negative integer.

A value of:

<break time="0" />

represents a zero-duration break.

Although syntactically valid, a zero-duration break normally has no observable effect.

Negative values are invalid.

6. Break Between Lines

A break may be placed between <line> elements.

Example:

<p>

    <line char="vestfal">
        — Это ракеты.
    </line>

    <break time="800" />

    <line char="narrator">
        Он на мгновение замолчал.
    </line>

</p>

The Player inserts the specified pause into the playback sequence.

7. Dramatic Pause

A break may be used to create a deliberate dramatic pause.

Example:

<p>

    <line char="narrator">
        Я посмотрел в окно.
    </line>

    <break time="1500" />

    <line char="narrator">
        И увидел то, чего никак не ожидал.
    </line>

</p>

The break is part of the content timing and therefore represents an intentional editorial decision.

8. Break and Speech

A <break> is not part of the spoken text.

For example:

<line char="narrator">
    Он ответил.
</line>

<break time="1000" />

<line char="alex">
    Я согласен.
</line>

The TTS engine processes only the text of the <line> elements.

The Player applies the break between them.

A <break> therefore does not require a TTS provider.

9. Break and startDelay

A <break> and startDelay both affect timing, but have different semantic meanings.

Break

A break represents an explicit pause in the script:

<break time="1000" />

It is a visible part of the project timeline.

startDelay

A startDelay modifies when an individual element begins:

<line
    char="alex"
    startDelay="1000">
    Я согласен.
</line>

The delay belongs to that element.

These mechanisms SHOULD NOT be treated as interchangeable.

10. Break and startMode

A <break> does not have a startMode attribute.

Its position in the document determines its position in the sequential script flow.

For example:

<p>

    <line char="alex">
        Первая реплика.
    </line>

    <break time="1000" />

    <line char="alex">
        Вторая реплика.
    </line>

</p>

The second line follows the break in the script sequence.

11. Break and Media

A break may occur between media or text elements.

Example:

<p>

    <line char="narrator">
        Сначала наступила тишина.
    </line>

    <break time="2000" />

    <audio
        src="door-open"
        volume="1" />

</p>

The Player determines the exact interaction between the break and simultaneously scheduled media according to the timing model.

A break does not automatically stop, pause, or mute media that has already started.

It represents a pause in the sequential script flow, not a global pause command.

12. Break Does Not Stop Playback

The <break> element MUST NOT be interpreted as a global player pause.

It does not mean:

Pause Player

It means:

Wait for N milliseconds in the script sequence

Therefore:

<break time="2000" />

does not pause unrelated background media that is already playing.

This distinction is important for music, ambience, video, and other continuously running media.

13. Break Inside a Chapter

A break may be used anywhere in the script where a break element is permitted.

Example:

<chapter id="chapter-1">

    <scene>

        <p>

            <line char="narrator">
                The door opened slowly.
            </line>

            <break time="1200" />

            <audio src="door-open" />

            <break time="500" />

            <line char="narrator">
                Nobody was there.
            </line>

        </p>

    </scene>

</chapter>

This allows the author to explicitly control narrative rhythm.

14. Break and Scenes

A break belongs to the script flow in which it is declared.

Example:

<scene>

    <line char="narrator">
        The room became silent.
    </line>

    <break time="2000" />

    <line char="narrator">
        Then I heard a sound.
    </line>

</scene>

A break does not change the scene itself.

Scene properties such as color and atmosphere remain unchanged.

15. Break and Word-by-Word Text

A break is different from word-by-word timing.

For example:

<line
    char="narrator"
    wordByWord="true"
    wordByWordMode="single"
    wordDisplayDuration="300">

    One two three.

</line>

<break time="1000" />

wordDisplayDuration controls the presentation timing of words within the line.

<break> controls the explicit pause after the line.

16. Break and Subtitles

A break contains no text and therefore does not generate a subtitle by itself.

For example:

<line char="narrator">
    Wait here.
</line>

<break time="2000" />

<line char="narrator">
    I'm coming back.
</line>

The Player MAY represent the silent interval visually, but the OVML Standard does not require a subtitle or placeholder to be displayed during a break.

17. Multiple Consecutive Breaks

Multiple breaks MAY appear consecutively:

<break time="500" />
<break time="1000" />

Their durations are additive:

500 ms + 1000 ms = 1500 ms

However, authors SHOULD normally prefer a single equivalent break:

<break time="1500" />

unless separate breaks have semantic significance for tooling or editing.

18. Runtime Behavior

The Player SHOULD treat a break as a deterministic timing event.

Conceptually:

Previous content
       │
       ▼
   <break>
       │
       │ wait N milliseconds
       ▼
Next content

The Player is responsible for implementing this behavior using the timing and playback mechanisms of the target platform.

The OVML document does not prescribe the internal implementation.

19. Validation

A validator SHOULD verify:

that <break> is syntactically valid;
that time is present;
that time is an integer;
that time is non-negative;
that the element does not contain child content.

Example of valid syntax:

<break time="1000" />

Invalid examples:

<break />
<break time="-500" />
<break time="one-second" />
<break time="1000">
    text
</break>

The validator is responsible for structural correctness.

The Player is responsible for runtime behavior.

20. Design Principle

The <break> element provides an explicit and portable way to express silence or inactivity in the sequential script flow.

It should be understood as:

A declarative request to introduce a defined duration of time between elements of the script.

It is not:

a TTS instruction;
a media pause command;
a Player pause command;
an audio asset;
a scene transition.

This distinction keeps OVML focused on describing what should happen and when, while leaving playback implementation to the Player.

21. Summary

The minimal form is:

<break time="1000" />

where:

time = duration in milliseconds

The <break> element:

contains no content;
does not require an asset;
does not require TTS;
participates in sequential script timing;
does not globally pause the Player;
may be used to control narrative rhythm;
is independent from startDelay;
is resolved by the Player at runtime.

<break> describes a pause in the script timeline, not a command to pause the Player.