# World Canon — `<world>`

> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

**OVML Standard 2.2**

## 1. Purpose

The `<world>` element defines the canonical entities of the project's world.

It is a document-level container, a sibling of `<cast>`, `<assets>`, and `<script>` under the root `<ovml>` element.

The world canon is declared once at the top of the document and acts as the single source of truth for the entities that recur across the project.

Script scenes reference a canonical entity by its `id` instead of duplicating the full description in every scene.

This keeps long-form projects consistent across many scenes and chapters.

The world canon is not limited to narrative content. The same mechanism serves any content whose entities recur and must stay consistent — a novel's locations, a lecture's terms, a documentation set's definitions and versions.

## 2. Canon, Not Copy

Long-form content repeats entities. Without a canon, every repetition invites drift:

- a location described as "dark" in one scene and "bright" in the next;
- a lecture term defined one way in the intro and another way in a later lesson;
- a prop that appears in one scene but is forgotten in the next.

The world canon solves this by describing each entity once and letting scenes refer to it.

## 3. A Generic Container With Free Sections

`<world>` does not prescribe a fixed set of sections.

A document declares the sections that its content requires.

`` `world
│
├── locations
│   └── location
│
├── timeline
│   └── era
│
├── factions
│   └── faction
│
├── terms
│   └── term
│
└── ... (any other section)
`` `
Every section follows the same structural rule.

## 4. The Uniform Section Rule

A section is a container of named entities.

An entity:

- has a stable `id`;
- has a display `name`;
- may contain arbitrary clarifying child elements;
- is referenced from script content by `ref`.

The parser does not need to know the kind of content to interpret a section. It applies the same rule to every section.

This makes the language self-describing. No project-type declaration is required.

## 5. Document Position

The `<world>` element is placed at the top of the document, before `<script>`:

`` `<ovml version="2.2" lang="en">

    <meta>
        ...
    </meta>

    <cast>
        ...
    </cast>

    <assets>
        ...
    </assets>

    <world>
        ...
    </world>

    <script>
        ...
    </script>

</ovml>
`` `
The `<world>` element is optional.

A document without a world canon is valid.

## 6. Entities in a Section

Each canonical entity is an element inside a section.

### Common attributes

Attribute	Type	Required	Description
`id`	string	yes	Stable key; referenced by scenes via `ref`
`name`	string	yes	Display name of the entity

### Example — locations section

`` `<world>
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
`` `
### Example — terms section (lecture, documentation)

`` `<world>
    <terms>
        <term
            id="photosynthesis"
            name="Photosynthesis">

            <definition>The process by which light is converted into chemical energy.</definition>
            <section>Biology, chapter 3</section>

        </term>
    </terms>
</world>
`` `
The child elements of an entity are free-form and project-specific. The standard only requires `id` and `name`.

The complete model for the locations section is defined in:

reference/locations.md

## 7. Referencing a Canon Entity From a Scene

A scene references a canonical entity via a child element with a `ref` attribute.

`` `<scene
    color="#1a1a2e"
    atmosphere="tense, night">

    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>

    ...
</scene>
`` `
The `ref` value is the `id` of an entity declared in the corresponding section of `<world>`.

The location-specific reference model is defined in:

reference/locations.md

The mechanism is identical for every section:

`` `<scene>
    <term ref="photosynthesis" />
    ...
</scene>
`` `
## 8. `<variation>`

A scene may specify how a referenced entity differs from its canon in this particular scene.

`` `<location ref="rusty_anchor">
    <variation>
        <time>night</time>
        <weather>foggy</weather>
        <changes>Chairs stacked, a fire in the hearth</changes>
    </variation>
</location>
`` `
Variation never modifies the canon.

Location-specific variation fields are defined in:

reference/locations.md

## 9. Backward Compatibility

A plain-text entity reference without `ref` remains valid.

For example:

`` `<scene>
    <location>A dark forest. Mist between the trees.</location>
    ...
</scene>
`` `
Existing documents therefore remain valid.

## 10. The Canon and the AI Assistant

The world canon gives the AI Assistant consistent memory of the project's recurring entities.

When creating a new scene, the Assistant:

- reads the canon to learn an entity's permanent description;
- references it by `ref`;
- adds a `<variation>` for anything different in this scene;
- keeps the canon itself unchanged.

This is the mechanism that keeps generated content consistent across a long project — whether the project is a novel, a course, or a documentation set.

## 11. Relation to Project Forms

The world canon is independent of any particular kind of content.

The standard does not define "project types". A parser interprets a document from its structure: whichever sections are present are parsed by the uniform section rule.

Applications may map the resulting structure to their own product categories, but that mapping is outside the standard.

## 12. Related Documents

See: reference/locations.md
See: reference/scene.md
See: reference/document.md