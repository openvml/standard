> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# `<line>` — Text and Dialogue Line

**OVML Standard 2.2**

## 1. Purpose

The `<line>` element represents a single textual content unit within a `<p>` block.

A line may represent:

- spoken dialogue;
- narration;
- a descriptive passage;
- a voice-over fragment;
- any other textual content that may be displayed, timed, and optionally synthesized as speech.

A line may reference a character through the `char` attribute.

A `<p>` may contain multiple `<line>` elements and may alternate between different characters.

For example:

```
<p>
    <line char="vestfal">
        — Это ракеты,
    </line>

    <line char="narrator">
        — раздался в моей голове чуть насмешливый голос моего спутника —
        дракона Вестфаля.
    </line>

    <line char="vestfal">
        — А если точнее, «Гроза», «Гарпия» и несколько «Ос»
        старой модификации,
    </line>

    <line char="narrator">
        — с нотками знатока добавил он.
    </line>
</p>

The line element therefore represents a single timed and optionally voiced text unit, not necessarily an entire paragraph or dialogue exchange.

2. Structure

The simplest form is:

<line char="alex">
    Hello!
</line>

A line may contain optional attributes controlling:

character assignment;
emotional direction;
speech rate;
timing;
text formatting;
word-by-word presentation;
marquee presentation;
grid placement;
transitions;
keyframes.
3. Character Reference

The char attribute identifies the character or voice assigned to the line.

<line char="alex">
    Hello!
</line>

The value MUST correspond to a character/@id defined in the document's <cast> section.

Example:

<cast>

    <character
        id="alex"
        name="Alex"
        gender="male"
        role="main" />

</cast>

<script>

    <p>
        <line char="alex">
            Hello!
        </line>
    </p>

</script>

The char attribute is optional only when the implementation allows text content that does not require character assignment.

If speech synthesis is requested, a character or voice MUST be resolvable by the consuming application.

4. Text Content

The textual content of <line> is the text represented by the line.

Example:

<line char="alex">
    This is the content of the line.
</line>

The text MAY contain Unicode characters and MAY use the language specified by the OVML document.

The textual content is preserved as authored content.

Applications MAY perform rendering-specific transformations such as:

word segmentation;
line wrapping;
text layout;
subtitle formatting;
speech synthesis.

Such transformations MUST NOT change the semantic text content of the OVML document.

5. Emotion

The emotion attribute describes the emotional or expressive direction of the line.

Example:

<line
    char="vestfal"
    emotion="sarcastic">
    — с нотками знатока добавил он.
</line>

Another example:

<line
    char="alex"
    emotion="frightened">
    What was that?
</line>

The value is a string rather than a fixed enumeration.

This allows different applications and voice systems to use their own expressive vocabularies.

Examples include:

happy
sad
angry
frightened
surprised
sarcastic
calm
excited
whispering
restrained anger

The OVML Standard does not define a universal emotion taxonomy.

The consuming application MAY interpret the value as:

a TTS emotion request;
a voice-direction hint;
a rendering hint;
metadata for an AI-assisted workflow;
information for human actors or editors.

A Player is not required to support every possible emotion value.

6. Speech Rate

The rate attribute defines the speech rate for the individual line.

Default:

1.0

Example:

<line
    char="alex"
    rate="1.2">
    Run!
</line>

The line-level rate overrides the character's base rate when both are present.

For example:

<character
    id="alex"
    name="Alex"
    rate="1.0" />

combined with:

<line
    char="alex"
    rate="1.25">
    Hurry up!
</line>

results in a requested speech rate of 1.25 for that line.

The exact interpretation of the value depends on the speech engine.

7. Timing

A line may specify when it begins relative to other content.

Supported timing attributes are:

Attribute	Type	Default	Description
startMode	enum	afterPrevious	Determines how the start time is interpreted
startTime	float	0	Start time in seconds
startDelay	float	0	Additional delay in seconds
7.1 startMode

The following values are defined:

Value	Description
afterPrevious	Starts after the preceding sequential text line or content block
absolute	Starts at the specified absolute timeline position
duringCurrent	Starts at the specified time relative to the current sequential context
afterPrevious
<line
    char="alex"
    startMode="afterPrevious">
    Next line.
</line>

The line begins after the preceding sequential content has completed.

This is the default mode.

It is particularly useful inside a <p> containing dialogue:

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

The lines form a sequential chain.

7.2 absolute

The absolute mode places the line at an absolute timeline position.

Example:

<line
    char="alex"
    startMode="absolute"
    startTime="12.5">
    This line starts at 12.5 seconds.
</line>

The value of startTime is measured in seconds from the relevant timeline origin.

7.3 duringCurrent

The duringCurrent mode starts the line at a specified time within the current sequential context.

Example:

<line
    char="alex"
    startMode="duringCurrent"
    startTime="2.5">
    This line starts 2.5 seconds into the current context.
</line>

The consuming application determines the exact runtime scheduling mechanism.

7.4 startDelay

startDelay adds an additional delay to the calculated start position.

Example:

<line
    char="alex"
    startMode="afterPrevious"
    startDelay="0.5">
    Delayed line.
</line>

The value is expressed in seconds.

8. Text Formatting

The following attributes control visual presentation of the line.

Attribute	Type	Default	Description
fontFamily	string	implementation-defined	Font family
fontSize	integer	implementation-defined	Font size in pixels
fontWeight	string/integer	implementation-defined	Font weight
fontColor	HEX/string	implementation-defined	Text color
textBackground	string	implementation-defined	Text background
textAlign	enum	left	Text alignment
textUppercase	boolean	false	Converts displayed text to uppercase
textShadow	string	implementation-defined	Text shadow definition
8.1 fontFamily

Defines the requested font family.

Example:

<line
    char="alex"
    fontFamily="Inter">
    Hello.
</line>

The font MUST be resolved by the consuming application.

A Player MAY substitute an equivalent font if the requested font is unavailable.

8.2 fontSize

Defines the requested font size in pixels.

Example:

<line
    char="alex"
    fontSize="32">
    Hello.
</line>
8.3 fontWeight

Defines the requested font weight.

Examples:

normal
bold
300
400
500
600
700
800
900

Example:

<line
    char="alex"
    fontWeight="700">
    Important!
</line>
8.4 fontColor

Defines the text color.

Example:

<line
    char="alex"
    fontColor="#ffffff">
    Hello.
</line>

Hexadecimal color notation SHOULD be used for portable color definitions.

8.5 textBackground

Defines the background applied to the rendered text.

Example:

<line
    char="alex"
    textBackground="#000000">
    Hello.
</line>

The exact rendering behavior is implementation-dependent.

8.6 textAlign

Defines text alignment.

Allowed values:

left
center
right
justify

Example:

<line
    char="alex"
    textAlign="center">
    Centered text.
</line>
8.7 textUppercase

Controls whether the displayed text is transformed to uppercase.

Default:

false

Example:

<line
    char="alex"
    textUppercase="true">
    This will be displayed in uppercase.
</line>

This affects presentation and MUST NOT alter the original semantic text stored in the document.

8.8 textShadow

Defines a text-shadow presentation.

Example:

<line
    char="alex"
    textShadow="2px 2px 4px rgba(0,0,0,0.7)">
    Shadowed text.
</line>

The value may use a CSS-compatible shadow representation.

The consuming application MAY adapt the value to its rendering system.

9. Word-by-Word Presentation

A line MAY be displayed progressively word by word.

The following attributes are defined:

Attribute	Type	Default	Description
wordByWord	boolean	false	Enables word-by-word display
wordByWordMode	enum	single	Determines how words remain visible
wordDisplayDuration	integer	implementation-defined	Display duration of a word in milliseconds
9.1 wordByWord

Example:

<line
    char="alex"
    wordByWord="true">
    Hello world!
</line>

When enabled, the consuming application progressively reveals the words of the line.

9.2 wordByWordMode

Supported values:

Value	Description
single	Only the current word is displayed
cumulative	Previously displayed words remain visible

Example:

<line
    char="alex"
    wordByWord="true"
    wordByWordMode="cumulative">
    Hello beautiful world.
</line>

The result progressively becomes:

Hello
Hello beautiful
Hello beautiful world.
9.3 wordDisplayDuration

Defines the requested display duration for each word in milliseconds.

Example:

<line
    char="alex"
    wordByWord="true"
    wordDisplayDuration="500">
    Hello world.
</line>

The consuming application MAY synchronize word display with generated speech timing instead of using this value as a fixed duration.

10. Marquee

The marquee attribute enables scrolling text presentation.

Default:

false

Example:

<line
    char="alex"
    marquee="true">
    This is a long scrolling line of text.
</line>

The exact movement, speed, and rendering of the marquee are implementation-dependent unless defined elsewhere in the standard.

11. Grid Placement

A line MAY specify its position and size within the project's visual grid.

Attribute	Type	Description
gridRow	integer	Grid row
gridCol	integer	Grid column
gridRowSpan	integer	Number of rows occupied
gridColSpan	integer	Number of columns occupied

Example:

<line
    char="alex"
    gridRow="1"
    gridCol="1"
    gridRowSpan="2"
    gridColSpan="4">
    Text positioned in the grid.
</line>

The consuming application determines the actual dimensions of the grid.

Automatic or implementation-defined placement MAY be used when these attributes are omitted.

12. Transitions

A line MAY define visual transitions when it appears or disappears.

The following attributes are defined:

Attribute	Type	Description
enter	transition	Entry transition
exit	transition	Exit transition

Supported transition values include:

none
fade
dissolve
cut
slide-left
slide-right
slide-up
slide-down
zoom-in
zoom-out

Example:

<line
    char="alex"
    enter="fade"
    exit="fade">
    A softly appearing line.
</line>

An implementation MAY expose additional transition types.

Unknown transition values SHOULD be handled according to the implementation's compatibility policy.

13. Keyframes

A line MAY contain keyframes defining changes to visual properties over time.

Example:

<line
    char="alex">

    Hello!

    <keyframes>
        <!-- keyframe definitions -->
    </keyframes>

</line>

Keyframes are intended for time-dependent visual changes.

Possible animated properties may include:

position;
scale;
opacity;
rotation;
size;
other renderer-supported properties.

The exact keyframe schema is defined by the OVML keyframe specification.

A Player that does not support a particular animated property MAY ignore that property while preserving the OVML document.

14. Speech and Presentation

Speech and visual presentation are related but independent aspects of a line.

For example:

<line
    char="vestfal"
    emotion="sarcastic"
    rate="0.9"
    fontSize="32"
    fontColor="#ffffff"
    enter="fade">

    — С нотками знатока добавил он.

</line>

The line contains:

Character
    → vestfal

Speech direction
    → sarcastic

Speech rate
    → 0.9

Visual presentation
    → 32px white text

Transition
    → fade

A consuming application may use all, some, or none of these capabilities depending on its supported feature set.

15. Character Defaults and Line Overrides

Character-level configuration provides defaults for the character.

Line-level configuration provides instructions specific to the current line.

For example:

<character
    id="alex"
    name="Alex"
    rate="1.0"
    pitch="1.0" />

and:

<line
    char="alex"
    rate="1.25">
    Run!
</line>

The effective speech rate for the line is:

line rate → 1.25

rather than the character's base rate of 1.0.

This model allows a character to maintain a consistent default voice while individual lines may be performed differently.

16. Lines Within <p>

A <p> element may contain multiple lines.

Each line may reference a different character.

Example:

<p>

    <line char="vestfal">
        — Это ракеты,
    </line>

    <line char="narrator">
        — с нотками знатока добавил он.
    </line>

</p>

The <p> provides the surrounding sequential context.

The paragraph model — including narration and dialogue roles, character defaults, and sequential chaining — is defined in:

reference/paragraph.md

17. Complete Example

A complete single-line example combining character, speech, and presentation attributes:

<line
    char="vestfal"
    emotion="sarcastic"
    rate="0.9"
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

    — С нотками знатока добавил он.

</line>

A complete multi-line paragraph example is defined in:

reference/paragraph.md
18. Validation

An OVML validator SHOULD verify:

char, when present, references an existing character;
startMode contains an allowed value;
startTime is numeric;
startDelay is numeric;
rate is numeric;
fontSize is a valid integer;
fontWeight, when numeric, is valid;
textAlign contains an allowed value;
boolean attributes contain valid boolean values;
wordByWordMode contains an allowed value;
grid values are valid integers;
transition values are valid according to the supported transition vocabulary.

Validation checks the structural correctness of the OVML document.

It does not determine:

whether a TTS provider can synthesize the line;
whether an emotion is supported by the selected voice;
whether a font exists;
whether a transition is supported by a particular Player;
whether a keyframe property is supported by a renderer.

Those are runtime or implementation concerns.

19. Design Principle

A <line> represents a single unit of textual content together with its optional speech, timing, and presentation instructions.

The line does not redefine the character.

Instead:

Character
    → defines who is speaking and the character's base voice

Line
    → defines what is being said
    → defines the emotional direction
    → may override speech parameters
    → defines timing
    → defines visual presentation

Therefore:

The character defines the speaker. The line defines the performance of that speaker at a particular point in the project.

This separation allows the same character to speak multiple lines with different emotions, speech rates, timings, and visual presentations.