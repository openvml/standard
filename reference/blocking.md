> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Blocking — `<blocking>`

**OVML Standard 2.2**

## 1. Purpose

The `<blocking>` element inside a `<scene>` describes the semantic relationships between the characters present in that scene.

Blocking records:

who is where;

who looks at whom;

who addresses whom;

who reacts to whom;

who enters or exits the scene.

Blocking describes intent and spatial relationships, not absolute coordinates or model-specific rendering instructions.

## 2. Structure

The `<blocking>` element is a child of `<scene>`.

It contains a list of `<character>` elements, each of which describes the positioning and relationships of one character.

Example:

<scene id="ch1_office" time="evening" mood="dramatic">

    <blocking>
        <character ref="anna" position="left" look_at="ivan" enters="true"/>
        <character ref="ivan" position="right" addresses="anna"/>
    </blocking>

</scene>

Each `<character>` entry references a character declared in the `<cast>` element by its `id`.

## 3. Attributes

Attribute	Type	Description
ref	string	charId of the character (from <cast>)
position	enum	Position: left, right, center, offscreen-left, offscreen-right
look_at	string	charId of the character this character looks at
addresses	string	charId of the character this character addresses speech to
reacts_to	string	charId of the character this character reacts to
enters	boolean	Whether the character enters the scene
exits	boolean	Whether the character exits the scene

## 4. ref

The `ref` attribute identifies the character being described.

Its value MUST correspond to an existing character `id` in the `<cast>` element.

<character ref="anna" position="left"/>

## 5. position

The `position` attribute describes the character's intended position relative to the scene.

Allowed values:

Value	Description
left	Character is on the left
right	Character is on the right
center	Character is centered
offscreen-left	Character is offscreen to the left
offscreen-right	Character is offscreen to the right

Position is a semantic hint, not a set of coordinates.

## 6. look_at

The `look_at` attribute identifies the character that this character looks at.

<character ref="anna" look_at="ivan"/>

The value is a charId.

## 7. addresses

The `addresses` attribute identifies the character that this character addresses speech to.

<character ref="ivan" addresses="anna"/>

The value is a charId.

## 8. reacts_to

The `reacts_to` attribute identifies the character that this character reacts to.

<character ref="anna" reacts_to="ivan"/>

The value is a charId.

## 9. enters and exits

The `enters` and `exits` attributes describe whether a character enters or leaves the scene.

<character ref="anna" position="left" look_at="ivan" enters="true"/>
<character ref="ivan" position="right" addresses="anna"/>

Both attributes are booleans.

A character may be marked as entering, exiting, or neither.

## 10. Example: Blocking

<scene id="ch1_office" time="evening" mood="dramatic">
    <location>Офис, неоновая подсветка</location>
    <characters>
        <char ref="anna"/>
        <char ref="ivan"/>
    </characters>
    <blocking>
        <character ref="anna" position="left" look_at="ivan" enters="true"/>
        <character ref="ivan" position="right" addresses="anna"/>
    </blocking>
    <prompt>S neon lights, dramatic lighting, two people facing each other</prompt>

    <p char="anna">Иван... ты слышал?</p>
    <p char="ivan">Слышал. Давай уходить.</p>
</scene>

## 11. Design Principle

OVML stores intent (semantic relations), not coordinates.

The runtime translates these relations into model-specific prompts.

For example:

look_at="ivan"

may become:

"left profile looking at person on right"

for one model, and:

"facing right, eyes on Ivan"

for another.

The OVML document therefore remains independent of any particular image, video, or rendering model.

## 12. Blocking and Prompt Generation

The `<prompt>` child of a scene may be supplied explicitly by the author.

Alternatively, it may be auto-generated from the `<blocking>` element.

Because blocking describes semantics rather than coordinates, an AI-assisted system can use it to produce appropriate model-specific prompts.

## 13. Blocking and Characters

The `<characters>` element of a scene lists the characters participating in the scene.

Example:

<characters>
    <char ref="anna"/>
    <char ref="ivan"/>
</characters>

Blocking then describes the relationships between those characters.

The `<character>` entries inside `<blocking>` and the `<char>` entries inside `<characters>` both reference cast character IDs.

## 14. Validation

An OVML validator SHOULD verify:

`ref` values inside `<blocking>` resolve to existing character IDs;

`position` contains an allowed value;

`enters` and `exits` contain valid boolean values;

`look_at`, `addresses`, and `reacts_to` contain valid character references when present.

Blocking is semantic metadata.

The validator does not determine whether the described staging is artistically appropriate.

## 15. Related Documents

See: reference/cast.md
See: reference/world.md
