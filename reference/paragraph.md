> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Paragraph — `<p>`

**OVML Standard 2.2**

## 1. Purpose

The `<p>` element is the base text block of an OVML script.

A `<p>` groups sequential textual content.

It is especially useful for dialogue and narration.

The `<p>` element contains the narration and dialogue lines of the project.

Each line is an individual `<line>` element.

Example:

<p>

    <line char="narrator">
        The room was silent.
    </line>

    <line char="alex">
        Is anyone here?
    </line>

    <line char="narrator">
        No one answered.
    </line>

</p>

## 2. Narration and Dialogue

A paragraph expresses two primary content roles:

narration — descriptive or narrative text, by default assigned to the narrator character;
dialogue — spoken text assigned to a named character.

In OVML 2.2 both roles are represented by the same mechanism: `<line>` elements inside `<p>`, with the char attribute selecting the speaker.

Narration:

<p>
    <line char="narrator">
        The sun rose over the horizon.
    </line>
</p>

Dialogue:

<p>
    <line char="hero" emotion="happy">
        What a wonderful day!
    </line>
</p>

The narrator is the default character.

When no char attribute is present at the paragraph or line level, the content is treated as narration.

## 3. Structure

The simplest paragraph is plain narration:

<p>
    Once upon a time...
</p>

A paragraph with an explicit character:

<p char="narrator">
    This story began long ago...
</p>

A paragraph MAY contain multiple `<line>` elements.

Different lines within the same paragraph MAY reference different characters.

The `<p>` element therefore represents a logical sequence, not necessarily a typographical paragraph.

## 4. Character Default

The char attribute sets the default character for the paragraph.

A `<line>` that does not specify its own char inherits the paragraph's character:

<p char="hero">

    <line>
        I said it once.
    </line>

    <line char="narrator">
        He said it again.
    </line>

</p>

The first line is assigned to hero by inheritance.

The second line overrides it explicitly with narrator.

If neither the line nor the paragraph specifies a char, the default is narrator.

## 5. Attributes

The following attributes may be applied to a `<p>` element.

Attribute	Type	Default	Description
char	string	narrator	Default character reference
emotion	string	implementation-defined	Emotional or expressive direction
rate	float	1.0	Speech rate
pitch	float	1.0	Voice pitch
volume	float	1.0	Requested loudness
timbre	string	implementation-defined	Voice timbre hint
emphasis	enum	implementation-defined	Emphasis scope (word, sentence, ...)
emphasis_level	enum	implementation-defined	Emphasis strength (low, medium, high)
intonation	enum	implementation-defined	Intonation direction (statement, question, ...)
marker	string	implementation-defined	Navigation marker

Example:

<p
    char="hero"
    emotion="angry"
    rate="1.2"
    emphasis="sentence"
    emphasis_level="medium">

    <line>
        Get out of my house!
    </line>

</p>

These attributes provide direction for speech synthesis and presentation.

The consuming application determines how they are interpreted.

Grid and text-presentation attributes are defined together with the line-level attributes in:

reference/line.md

## 6. Sequential Dialogue

When lines use the default:

startMode="afterPrevious"

they form a sequential chain.

Example:

<p>

    <line char="alex">
        Hello.
    </line>

    <line char="maria">
        Hello, Alex.
    </line>

    <line char="alex">
        How are you?
    </line>

</p>

The intended sequence is:

Alex
  ↓
Maria
  ↓
Alex

The Player determines how the individual audio segments are generated, buffered, and played.

## 7. Parallel Content

OVML also allows content inside a paragraph to overlap through explicit timing.

For example:

<p>

    <line
        char="narrator"
        startMode="absolute"
        startTime="0">
        The storm was approaching.
    </line>

    <audio
        src="storm"
        startMode="absolute"
        startTime="0"
        loop="true" />

</p>

Both elements begin at the same timeline position.

The script therefore expresses the relationship:

Narration ────────────────>
Storm audio ─────────────────────────>
            0s

The consuming Player is responsible for realizing the synchronization.

## 8. Paragraph and Timing

The paragraph provides a sequential context for its lines.

Line-level timing, text formatting, word-by-word presentation, grid placement, transitions, and keyframes are defined on the `<line>` element.

See: reference/line.md

## 9. Paragraph Within a Scene

A paragraph normally occurs inside a scene.

<scene atmosphere="quiet forest">

    <p>

        <line char="narrator">
            The forest was unusually quiet.
        </line>

        <line
            char="alex"
            emotion="whispering">
            Did you hear that?
        </line>

    </p>

</scene>

The scene provides the dramatic and visual context.

The paragraph provides the sequential text context.

## 10. Complete Example

The following example shows a paragraph with multiple lines, alternating speakers, emotions, speech rates, and text-presentation features:

<p>

    <line
        char="vestfal"
        emotion="calm"
        rate="1.0"
        startMode="afterPrevious"
        fontFamily="Inter"
        fontSize="32"
        fontWeight="500"
        fontColor="#ffffff"
        textBackground="#000000"
        textAlign="left"
        textUppercase="false"
        wordByWord="false"
        marquee="false"
        gridRow="1"
        gridCol="1"
        gridRowSpan="1"
        gridColSpan="4"
        enter="fade"
        exit="fade">

        — Это ракеты,

    </line>

    <line
        char="narrator"
        emotion="slightly amused"
        rate="1.0"
        startMode="afterPrevious">

        — раздался в моей голове чуть насмешливый голос
        моего спутника — дракона Вестфаля.

    </line>

    <line
        char="vestfal"
        emotion="confident"
        rate="0.95"
        startMode="afterPrevious"
        wordByWord="true"
        wordByWordMode="cumulative"
        wordDisplayDuration="100">

        — А если точнее, «Гроза», «Гарпия» и несколько «Ос»
        старой модификации,

    </line>

    <line
        char="narrator"
        emotion="knowing"
        startMode="afterPrevious">

        — с нотками знатока добавил он.

    </line>

</p>

## 11. Validation

An OVML validator SHOULD verify:

a `<p>` element is properly opened and closed;
`char`, when present, references an existing character;
lines are correctly enclosed within the paragraph;
boolean attributes contain valid boolean values;
timing and numeric attributes contain valid values.

Validation checks the structural correctness of the document.

It does not determine whether the paragraph reads well, whether a TTS provider can synthesize it, or whether an emotion is supported by the selected voice.

## 12. Design Principle

A `<p>` is a logical text block, not a typographical instrument.

It groups sequence of lines that belong together in the narrative or informational flow.

Paragraph
    └── defines the sequential context

Line
    ├── defines who speaks
    ├── defines what is said
    ├── defines how it is spoken
    └── defines how it is displayed

This separation allows the same paragraph structure to serve narration, dialogue, lectures, and interactive projects.

## 13. Related Documents

See: reference/line.md
See: reference/scene.md
See: reference/cast.md
See: reference/timing.md
See: reference/media.md