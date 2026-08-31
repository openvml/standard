> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# Locations — `<locations>` and `<location>`

**OVML Standard 2.2**

## 1. Purpose

The `<locations>` section of the world canon defines the canonical places in which the project's
scenes occur.

Each location is a `<location>` element inside `<locations>`.

A scene references a canonical location by its `id` instead of duplicating the full description.

This keeps long-form projects consistent across many scenes and chapters: a location described once
in the canon remains the same location everywhere it is referenced.

The mechanism is generic. The same world-canon section model serves a novel's locations, a lecture's
terms, and a documentation set's definitions. See: [`reference/world.md`](world.md)

## 2. The Locations Section

The `<locations>` section is a child of the project-level `<world>` element, declared once at the
top of the document alongside `<cast>` and `<assets>`.

Example:

```xml
<world>

    <locations>

        <location
            id="rusty_anchor"
            name="The Rusty Anchor"
            type="tavern">

            <era>fantasy-medieval</era>
            <style>rough-hewn oak, candlelight</style>
            <palette>hearth:#4a2f1a; walls:#3a2a1e</palette>
            <props>cracked bar, long benches, hearth</props>
            <atmosphere>low murmur, smell of ale and smoke</atmosphere>

        </location>

    </locations>

</world>
```

The canon holds the permanent properties of each location.

Scene-specific change (time of day, weather, temporary props) is expressed by a `<variation>` in the
scene, never by editing the canon.

## 3. Attributes

Attribute	Type	Required	Description
id	string	Yes	Stable key; referenced from scenes via ref
name	string	Yes	Display name of the location
type	string	No	Location type: tavern, forest, cave, palace, street, interior, exterior, and so on

Example:

```xml
<location
    id="rusty_anchor"
    name="The Rusty Anchor"
    type="tavern">
```

    ...

```xml
</location>
```

The `id` is the stable key used by scene references.

The `name` is the human-readable display name.

The `type` is a free-form descriptive category; the standard does not fix its vocabulary.

## 4. Child Elements

A location may contain arbitrary clarifying child elements.

Common canonical fields include:

Tag	Description
<era>	Epoch / historical period
<style>	General visual style (materials, finish)
<palette>	Color palette (for UI/rendering)
<props>	Permanent props and objects of the location
<atmosphere>	Permanent atmosphere (smells, sounds, light)

The child elements of a location are free-form and project-specific.

The standard only requires `id` and `name`.

## 5. Canon, Not Copy

Long-form content repeats locations.

Without a canon, every repetition invites drift:

- a tavern described as "dark" in one scene and "bright" in the next;
- a prop that appears in one scene but is forgotten in the next;
- an atmosphere that changes meaning from scene to scene.

The world canon solves this by describing each location once and letting scenes refer to it by `id`.

## 6. Referencing a Location From a Scene

A scene references a canonical location through a child <location> element with a ref attribute.

```xml
<scene
    color="#1a1a2e"
    atmosphere="tense, night">

    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>
```

```xml
    ...
</scene>
```

Attribute	Type	Description
ref	string	The id of a location in the <locations> section of <world>

The scene therefore does not repeat the location's full description.

It points to the canon and adds only what is different in this scene.

## 7. <variation>

A scene may specify how a referenced location differs from its canon in this particular scene.

```xml
<location ref="rusty_anchor">
    <variation>
        <time>night</time>
        <weather>foggy</weather>
        <changes>Chairs stacked, a fire in the hearth</changes>
    </variation>
</location>
```

Common variation fields:

Tag	Description
<time>	Time of day in the scene: morning, afternoon, evening, night
<weather>	Weather in the scene: clear, cloudy, rainy, foggy, snowy
<changes>	Free-form scene-specific changes (broken items, added props)
<atmosphere>	Scene-specific atmosphere (overrides the canon)

Variation never modifies the canon.

The canon remains the permanent description; the variation describes the transient state of this
scene.

## 8. Free-form Location Description

A plain-text <location> without ref remains valid.

```xml
<scene>
    <location>A dark forest. Mist between the trees.</location>
    ...
</scene>
```

Such a description is a free-form scene-local location without a canonical binding.

Existing documents therefore remain valid.

## 9. Locations and Illustration

A scene may combine a location reference with an image or video that depicts it.

```xml
<scene>

    <location ref="rusty_anchor" />

    <img
        src="tavern-interior"
        layer="background" />

    <audio
        src="tavern-ambience"
        volume="0.4"
        loop="true" />

</scene>
```

The canon describes the location semantically.

The media describes how the location is visualized and heard in this scene.

## 10. The Canon and the AI Assistant

The world canon gives the AI Assistant consistent memory of the project's recurring locations.

When creating a new scene, the Assistant:

- reads the canon to learn a location's permanent description;
- references it by ref;
- adds a <variation> for anything different in this scene;
- keeps the canon itself unchanged;

This is the mechanism that keeps generated content consistent across a long project.

## 11. Validation

An OVML validator SHOULD verify:

- <location> elements are properly nested inside <locations>;
- id and name are present;
- ref values in scenes resolve to existing location ids when project context is available;
- variation fields contain valid values;
- child elements are allowed in their respective contexts.

The validator does not determine whether a location description is good, appropriate, or
atmospherically correct.

## 12. Design Principle

Locations hold permanent properties; scenes hold variations.

A location is described once in the canon and referenced many times by id.

This separation keeps descriptions consistent, reduces duplication, and gives the Player and the AI
Assistant a single stable record of where a story's action takes place.

## 13. Related Documents

See: [`reference/world.md`](world.md)
See: [`reference/scene.md`](scene.md)
See: [`reference/assets.md`](assets.md)
See: [`reference/document.md`](document.md)
