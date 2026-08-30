# OpenVML Audiobook Example

This example demonstrates how a **narrated audiobook** is expressed as an
OpenVML document. It is a companion to the first application example
(`docs/standard/examples/ner_test.ovml`) and the plain-text source manuscript
in `source.txt`.

The project is a short narrated story, *The Lantern Keeper and the Lost Bell*,
featuring a narrator and three characters.

## Purpose

The document shows how an audiobook is expressed in OVML using the standard's
core building blocks:

* `meta` — project title and author;
* `cast` — a narrator plus named characters, each with a voice assignment;
* `assets` — reusable audio resources (background music, sound effects);
* `script` — the timeline, split into `chapter` → `scene` → blocks.

The audiobook content itself is **narrated text with dialogue**, not embedded
audio: the TTS voices are *referenced* on the `cast` characters and resolved by
the Player at runtime.

## Narration vs. Dialogue

OVML distinguishes narration from dialogue through the `char` attribute on
`<line>` (or `<p>`) elements:

* Narration is assigned to `narrator`;
* Dialogue is assigned to a named character.

``xml`
<p>
  <line char="narrator">The old man studied her for a long moment.</line>
  <line char="jonas" intonation="statement">That bell isn't mine to ring.</line>
</p>
```

Speech directions such as `emotion` and `intonation` may be added sparingly to
dialogue lines as a hint to the synthesis engine.

## Timing

Pauses are expressed with the `<break>` element between paragraphs or lines:

``xml`
<p>
  <line char="narrator">Every evening at dusk he climbed the iron stairs...</line>
</p>
<break duration="600"/>
<p>
  <line char="narrator">One autumn night...</line>
</p>
```

Ambient music and sound effects are placed on the same timeline as `<audio>`
blocks:

``xml`
<audio src="#music" action="background" volume="0.2"/>
<audio src="#chapel_bell" action="sfx" volume="0.5"/>
```

## TTS Voice References (no embedded audio)

The cast declares the voices, but no spoken audio is embedded in the document.
Each character references a voice engine, voice name, and language:

``xml`
<character id="jonas" name="Jonas" color="#d2691e"
  role="major" gender="male" age="elderly"
  voiceEngine="edge_tts"
  voiceName="Deep, rumbling, and weathered."
  voiceLang="en-US"
  pitch="0.9" rate="0.95"/>
```

The Player synthesizes the speech from the line text at runtime, so the same
script can be spoken by any TTS engine that can supply the requested voices.

## Re-targeting the Same Document

This example is written with an audiobook intent, but OVML describes **intent,
not output**. The same document could be re-targeted by the runtime to other
forms — for example, a video presentation with subtitles and background images,
or an interactive experience — without changing the underlying script structure.
The runtime decides how the content is buffered, synthesized, decoded, rendered,
and synchronized on the target platform.

## Relation to the Standard

This directory is an informative example. It does not redefine the OpenVML
language. Normative definitions of the elements, attributes, timing rules,
media behavior, cast properties, and processing presets are specified under:

``text`
docs/standard/concepts/
docs/standard/reference/
docs/standard/presets/
```

The example should be read together with those reference documents.
