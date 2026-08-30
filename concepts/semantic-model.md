# OpenVML Semantic Model

**OpenVML Standard 2.2**

This document explains the conceptual model of meaning in OVML: a scene stores intent — desired relationships, blocking, emotion, atmosphere — rather than pixel or output primitives. The same idea appears throughout the standard in different forms.

Understanding this model is the key to knowing why OVML looks the way it does and why the same document can be realized so differently by different applications.

1. Intent, Not Output

The core of the semantic model is that OVML records what the author wants, not the finished pixels or samples.

A traditional media format describes a final result. OVML describes the structure and intentions of the work. The distinction is not cosmetic: it is what allows the same source to be projected onto many renderers.

A scene is the natural place to see this. A scene groups content that belongs to the same creative context, but it does not perform rendering. It describes what belongs together from the author's point of view, and it leaves the rendering engine, codec, audio backend, and other implementation details to the Player.

2. The Layering of Intent

Meaning in OVML is expressed at several levels. These levels are not competing; they are complementary, and a single scene frequently combines all of them.

    Literal text — the actual lines spoken or shown. Dialogue and narration are the concrete, editable text of the work.

    Semantic — the desired relationships, blocking, emotion, atmosphere, and camera intent. These describe meaning and direction, not coordinates.

    Procedural — the media elements and processing presets used to realize the scene. These are concrete but still declarative: they describe what media and processing to use, not how to render it.

3. Literal Text: Lines

At the literal level, a scene contains <line> elements that carry dialogue or narration:

    <scene atmosphere="night city">

        <line char="alex">
            Look at the lights.
        </line>

    </scene>

A line is spoken text. It may reference a character, carry timing information, and include word-level markup. It is the most concrete and immediately meaningful part of the document: this is what is said.

4. Semantic Intent: Blocking

At the semantic level, a scene may describe the intended relationships between the characters present in it through <blocking>.

Blocking records who is where, who looks at whom, who addresses whom, and who enters or exits the scene:

    <scene>
        <blocking>
            <character ref="anna" position="left" look_at="ivan" enters="true" />
            <character ref="ivan" position="right" addresses="anna" />
        </blocking>
    </scene>

These are semantic relations, not coordinates. The value position="left" does not say "x=10, y=200"; it says the character is intended to be on the left. A runtime translates these relations into whatever it needs, and different models or renderers may translate them differently — into a prompt for one system, a layout rule for another.

The standard stores intent, not coordinates, precisely so that the runtime can interpret the relations for its own model.

5. Semantic Intent: Emotion and Atmosphere

The same principle applies to emotion and atmosphere.

A scene carries an atmosphere attribute that describes its mood as free-form text:

    <scene atmosphere="тревога, дождь, пустая улица">
        ...
    </scene>

This is semantic information, not a rendering command. It does not mean the Player must add rain, apply a dark filter, or generate a sound effect. It describes the author's intent, which an AI Assistant, authoring tool, or asset-selection system may use as a hint.

On a line, the emotion attribute describes the emotional state of the speaker:

    <line char="alex" emotion="angry">
        Get out of here!
    </line>

As with atmosphere, emotion is a hint about intended delivery. The TTS engine or Player decides how it is realized.

6. Semantic Intent: Camera

Camera instructions extend the same model to visual direction. A <camera> element is a directorial instruction, not a resource:

    <camera
        shot="close"
        framing="center"
        target="alex" />

It says how the current composition should be framed and how attention should move, without prescribing a rendering API. Different renderers realize the same instruction with different graphics or video technology.

The camera participates in the same intent-driven model as everything else: OVML describes the camera intent; the renderer decides how to realize it.

7. Procedural Intent: Media and Processing

The procedural level is where concrete resources appear. Media elements are still instructions, not files:

    <video
        src="background"
        layer="background"
        duration="10" />

The asset is declared in <assets>, and the media element instructs how that asset is used at this point in the timeline — when it starts, how long it lasts, where it is placed, how large it is.

Processing presets are the same idea applied to the audio and visual signal. A preset is a declarative, non-destructive list of operations applied to a source at runtime. It describes what processing is wanted, not the DSP or rendering implementation.

8. Why the Layering Matters

The separation of literal, semantic, and procedural intent has practical consequences.

    Literal text can be edited without touching visuals or processing.

    Semantic intent survives changes in assets. Replacing an image or changing a voice does not change the blocking or the atmosphere.

    Procedural instructions can be reinterpreted by different renderers, so the same document works across engines and platforms.

This is what makes OVML suitable for long-form, edited, translated, and re-voiced content: the meaning of the work is preserved in intent, not locked into a single rendering decision.

9. Summary

The semantic model of OVML is one idea repeated everywhere:

a scene stores intent, not output primitives.

    Lines are the literal text.
    Blocking, emotion, atmosphere, and camera carry the semantic meaning.
    Media and processing carry the procedural, still-declarative realization.

The runtime interprets all of these for its own model. Because the document stores intent rather than coordinates, the same OVML project can be rendered, read, synthesized, and analyzed by entirely different applications.

See: reference/blocking.md
See: concepts/scenes-and-world.md
See: concepts/camera.md
