> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# `<script>` — Project Script

**OVML Standard 2.2**

## 1. Purpose

The `<script>` element contains the primary audiovisual content of an OVML document.

It defines the logical structure of the project and organizes content into chapters, scenes, and content blocks.

The `<script>` element describes **what happens and when it should happen**.

It does not prescribe how the content is:

- streamed;
- buffered;
- synthesized;
- decoded;
- rendered;
- accelerated;
- synchronized with a particular hardware device.

Those responsibilities belong to the consuming application or Player.

The basic structure is:

```
<script>

    <chapter>
        <scene>

            <p>
                <line char="narrator">
                    Once upon a time...
                </line>
            </p>

            <video src="forest-video" />
            <audio src="forest-ambience" />

        </scene>
    </chapter>

</script>
2. Script Hierarchy

The logical hierarchy of an OVML script is:

<script>
    │
    ├── <chapter>
    │     │
    │     ├── <scene>
    │     │     │
    │     │     ├── <camera>
    │     │     │
    │     │     ├── <p>
    │     │     │     └── <line>
    │     │     │
    │     │     ├── <video>
    │     │     ├── <img>
    │     │     ├── <audio>
    │     │     └── <break>
    │     │
    │     └── ...
    │
    └── ...

The hierarchy provides different levels of semantic organization:

Element	Purpose
<script>	Complete project script
<chapter>	Major navigable section
<scene>	Continuous dramatic/visual context
<camera>	Camera/view direction within a scene
<p>	Sequential textual/dialogue block
<line>	Individual textual/speech unit
<video>	Video media
<img>	Image media
<audio>	Audio media
<break>	Explicit pause
3. <script>

The <script> element is the root container for the project's content.

Example:

<script>

    <chapter>
        ...
    </chapter>

    <chapter>
        ...
    </chapter>

</script>

A script MAY contain one or more chapters.

A script without chapters MAY be supported by implementations for simple projects, but portable OVML documents SHOULD use <chapter> elements when the content has meaningful navigational sections.

4. Chapters

A <chapter> represents a major logical section of the project.

It contains scenes and content blocks and provides a natural navigation point for Players.

The complete chapter model, including chapter metadata, is defined in:

reference/chapter.md

5. Scenes

A chapter MAY contain multiple <scene> elements.

A scene represents a continuous logical audiovisual context.

Example:

<chapter>

    <scene color="#1a1a2e" atmosphere="quiet evening">

        ...

    </scene>

    <scene color="#3b1f1f" atmosphere="danger and tension">

        ...

    </scene>

</chapter>

Scenes provide a semantic boundary between different environments, moods, locations, or dramatic situations.

Scene semantics are defined in:

reference/scene.md

6. Camera

A scene MAY contain a <camera> element.

The <camera> element represents the intended camera or viewpoint configuration for the scene.

Example:

<scene>

    <camera
        type="close-up"
        target="vestfal" />

    ...

</scene>

Camera semantics are defined in:

reference/scene.md
concepts/camera.md

Camera instructions are declarative.

They describe the intended viewpoint rather than prescribing the implementation of the rendering pipeline.

7. Paragraphs and Dialogue

A <p> element groups sequential textual content, especially dialogue and narration.

It MAY contain multiple <line> elements, and different lines within the same paragraph MAY reference different characters.

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

When lines use the default startMode="afterPrevious", they form a sequential chain:

Alex
  ↓
Maria
  ↓
Alex

The complete paragraph model, including narration and dialogue roles, is defined in:

reference/paragraph.md

8. Parallel Content

OVML also allows content to overlap through explicit timing.

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

9. Media Content

A scene MAY contain media elements.

Supported media elements include:

<video />
<img />
<audio />

Example:

<scene>

    <video
        src="forest"
        startMode="absolute"
        startTime="0" />

    <audio
        src="forest-ambience"
        startMode="absolute"
        startTime="0"
        loop="true" />

    <p>

        <line char="narrator">
            The forest was silent.
        </line>

    </p>

</scene>

Media semantics are defined in:

reference/media.md
10. Explicit Pauses

A script MAY contain explicit pauses using <break>.

Example:

<p>

    <line char="narrator">
        He opened the door.
    </line>

    <break time="1000" />

    <line char="narrator">
        And froze.
    </line>

</p>

The pause expresses intentional temporal separation between content.

The <break> element does not represent silence as an audio asset.

Its semantics are defined in:

reference/break.md
11. Script Timing

OVML timing is declarative.

Content may use:

afterPrevious
duringCurrent
absolute

For example:

<line
    char="alex"
    startMode="afterPrevious">
    First line.
</line>

<line
    char="maria"
    startMode="duringCurrent"
    startTime="2.0">
    Overlapping line.
</line>

<audio
    src="music"
    startMode="absolute"
    startTime="0" />

The script defines the intended temporal relationships.

The Player determines how those relationships are implemented at runtime.

12. Nested Timing Context

Timing is interpreted within the logical context in which an element appears.

For example:

<chapter>

    <scene>

        <p>

            <line
                char="alex"
                startMode="afterPrevious">
                Hello.
            </line>

            <line
                char="maria"
                startMode="afterPrevious">
                Hi.
            </line>

        </p>

    </scene>

</chapter>

The lines form a sequential chain inside the paragraph.

By contrast:

<chapter>

    <scene>

        <line
            char="alex"
            startMode="absolute"
            startTime="10">
            Hello.
        </line>

        <audio
            src="music"
            startMode="absolute"
            startTime="10" />

    </scene>

</chapter>

both elements are scheduled against the scene's timeline.

The precise runtime scheduling model is the responsibility of the Player.

13. Script as a Timeline Description

An OVML script should be understood as a declarative timeline description.

For example:

<script>

    <chapter>

        <scene atmosphere="quiet forest">

            <audio
                src="forest"
                startMode="absolute"
                startTime="0"
                loop="true" />

            <p>

                <line
                    char="narrator"
                    startMode="absolute"
                    startTime="0">

                    The forest was unusually quiet.

                </line>

                <line
                    char="alex"
                    startMode="afterPrevious"
                    emotion="whispering">

                    Did you hear that?

                </line>

            </p>

        </scene>

    </chapter>

</script>

The semantic intent is:

0s
│
├── Forest ambience starts
│
└── Narrator starts
        │
        ▼
    Narrator finishes
        │
        ▼
    Alex speaks

The OVML document does not specify:

which audio decoder is used;
how much audio is buffered;
whether TTS is generated locally or remotely;
whether audio is streamed;
which rendering API is used;
which hardware acceleration is available.
14. TTS and Script Execution

A line assigned to a character MAY require speech synthesis.

For example:

<line char="alex">
    Welcome to the project.
</line>

The character definition may provide the requested voice:

<character
    id="alex"
    name="Alex"
    voiceEngine="edge-tts"
    voiceId="en-US-GuyNeural"
    voiceLang="en-US" />

For a plain .ovml document, the Player MAY synthesize the speech at playback time.

For an OVMZ package, synthesized audio MAY already be included in the package.

For an OVMV package, the audiovisual result may already be rendered into a final video.

The script remains the semantic source describing the content and its timing.

15. External Assets

An OVML script MAY reference media assets externally.

Example:

<video
    src="https://example.com/video/forest.mp4" />

or through an asset identifier:

<video src="forest-video" />

The interpretation of an asset reference depends on the asset definition and the consuming application.

External assets MAY include:

images;
video;
audio;
subtitles;
other supported media resources.

The Player is responsible for retrieving, validating, buffering, and decoding external resources when permitted.

16. Script and Package Forms

The same logical script may exist in different delivery forms.

Plain OVML
project.ovml

The document contains the script and references resources externally or through supported resource identifiers.

The Player may need to:

resolve assets;
retrieve media;
synthesize speech;
decode media;
render the project.
OVMZ
project.ovmz

The script is packaged together with the resources required for the project.

Example:

project.ovmz/
├── content.ovml
├── project.json
├── resources/
├── presets/
└── tts/

The Player can therefore use pre-generated TTS and bundled media.

OVMV
project.ovmv

The audiovisual result has already been rendered into a final video representation.

The Player does not need to reconstruct the original TTS or media composition in order to play the final video.

The complete package model is defined in the package documentation.

17. Navigation

Chapters provide logical navigation points.

A Player MAY expose:

Chapter 1
Chapter 2
Chapter 3
...

Navigation MAY allow the user to:

move to the next chapter;
move to the previous chapter;
select a chapter;
resume from a chapter;
display chapter progress.

OVML does not require a specific user-interface representation.

18. Accessibility

The semantic structure of <script>, <chapter>, <scene>, <p>, and <line> may be used by Players to provide accessibility features.

Possible applications include:

screen-reader navigation;
chapter navigation;
subtitle presentation;
text-to-speech;
adjustable playback rate;
dialogue identification;
searchable transcripts.

Accessibility behavior is implementation-dependent unless explicitly defined elsewhere in the standard.

19. Validation

An OVML validator SHOULD verify:

the <script> element is structurally valid;
chapters and scenes are properly nested;
all XML elements are properly opened and closed;
<line char="..."> references a valid character;
timing attributes contain valid values;
child elements are allowed in their respective contexts;
media references use the required syntax;
numeric values are valid numbers;
boolean attributes contain valid boolean values.

The validator verifies the syntax and structural consistency of the document.

It does not attempt to determine whether the director's creative decisions are good, logical, aesthetically appropriate, or technically optimal.

For example, the following may be structurally valid:

<line
    char="alex"
    startMode="absolute"
    startTime="1000">
    Hello.
</line>

even if the author intended 1000 milliseconds rather than 1000 seconds.

Semantic or creative intent beyond the defined data model is outside the validator's responsibility.

20. Player Responsibilities

The Player interprets the declarative script and turns it into an executable presentation.

Depending on the project form, the Player may:

OVML
 │
 ├── resolve characters
 ├── resolve assets
 ├── resolve voices
 ├── synthesize TTS
 ├── decode media
 ├── schedule content
 ├── synchronize audio/video/text
 ├── apply transitions
 ├── apply camera instructions
 └── render the result

Different Players may implement these operations differently while consuming the same OVML document.

21. Design Principle

OVML is intentionally declarative.

The script describes the intended content and temporal relationships without prescribing the implementation.

Therefore:

OVML defines when content should occur. The Player determines how that content is buffered, streamed, synthesized, decoded, rendered, and synchronized on the target platform.

This separation allows the same timing model to be used for:

lectures;
presentations;
Shorts/Reels;
audiobooks;
game voiceover;
film dubbing;
anime;
courses;
podcasts;
interactive multimedia projects.

The same OVML script may therefore be consumed by different Players and rendering environments without changing the underlying content model.