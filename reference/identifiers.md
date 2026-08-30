> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Identifiers and References

**OVML Standard 2.2**

## 1. Purpose

OVML uses identifiers to give stable names to reusable entities and references to connect those entities to their uses.

An identifier is a stable key that names an entity.

A reference is an attribute that points back to that identifier from another place in the document.

This separation allows an entity to be declared once and referenced many times.

## 2. The Reference Model

OVML separates an entity from the instruction that uses it.

An identifier names the entity.

A reference identifies that entity from a use site.

For example:

<character id="alex" name="Alex" />

The `id` names the character.

The script references that character through the `char` attribute:

<line char="alex">
    Welcome.
</line>

Similarly, an asset:

Asset
  │
  │ identified by id
  ▼
Asset Reference (src)
  │
  ▼
Media Element

## 3. Stable IDs

Entities that are referenced by other parts of a document SHOULD carry a stable `id`.

Common identifiers include:

character id;
scene id;
chapter id;
location id;
asset id;
entity id in a world section.

An identifier should remain stable within the scope in which it is referenced.

Changing the physical resource or the display name of an entity does not necessarily change its identifier.

## 4. Uniqueness

An identifier MUST be unique within its resolution scope.

For example:

character IDs must be unique within the OVML document;

location IDs must be unique within the world canon;

asset IDs must be unique within the asset catalog;

scene and chapter IDs must be unique within the referenced scope.

Two entities in different scopes may use the same identifier value where their resolution scopes do not overlap.

## 5. Naming Conventions

The standard does not prescribe a single naming convention.

Identifiers SHOULD be stable across document revisions.

Examples:

<character id="vestfal" ... />
<location id="rusty_anchor" ... />
<scene id="ch1_tavern_night" ... />
<chapter id="ch_01" ... />
<asset id="img_hero_portrait" ... />

Common conventions use lowercase, underscores, or hyphens to keep identifiers readable and stable.

Identifiers for characters should preserve the original script identity.

Character IDs are not transliterated or freely translated; they retain the value used in the source script.

## 6. Referencing Characters

Characters are referenced by their `id` through the `char` attribute.

<line char="alex">
    Hello.
</line>

The value of `char` MUST correspond to an existing character `id` in the `<cast>` element.

See: reference/cast.md

## 7. Referencing World Entities

A scene references a canonical entity through a child element with a `ref` attribute.

<scene>
    <location ref="rusty_anchor">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>
</scene>

The `ref` value is the `id` of an entity declared in the corresponding section of `<world>`.

The mechanism is identical for every section:

<scene>
    <term ref="photosynthesis" />
</scene>

See: reference/world.md

## 8. The Uniform Section Rule

A world section is a container of named entities.

An entity:

has a stable `id`;

has a display `name`;

may contain arbitrary clarifying child elements;

is referenced from script content by `ref`.

The parser applies the same rule to every section.

This makes the language self-describing.

## 9. Referencing Locations

A location may be declared in the world canon and referenced from a scene.

<world>
    <locations>
        <location
            id="rusty_anchor"
            name="The Rusty Anchor"
            type="tavern">
            ...
        </location>
    </locations>
</world>

A scene then references it:

<location ref="rusty_anchor" />

A plain-text location reference without `ref` remains valid:

<location>A dark forest. Mist between the trees.</location>

## 10. Referencing Assets

Assets are referenced through the `src` attribute of a media element.

<video src="forest-video" />

The value may be:

an external URL;
a logical asset ID;
a package-local path;
another resource identifier defined by a compatible implementation.

Examples:

<img src="https://cdn.example.com/images/forest.jpg" />
<img src="forest-background" />
<img src="resources/images/forest.jpg" />

The reference form does not change the semantic role of the asset.

## 11. Logical Asset IDs

A project SHOULD prefer logical asset identifiers when the project has an asset catalog.

<video src="intro-video" />

The logical ID may resolve to:

https://cdn.example.com/video/intro.mp4

or:

resources/video/intro.mp4

or another supported resource.

This separation allows the resource location to change without changing the OVML script.

See: reference/assets.md

## 12. Asset Identifiers in the Catalog

An asset catalog associates a logical ID with resource information.

{
  "id": "forest-video",
  "type": "video",
  "location": "resources/video/forest.mp4"
}

The asset identity remains stable within the scope in which it is referenced.

Media element `src` values are references into the asset catalog.

## 13. Resolution Scope

Scopes include:

the character library (character IDs);

the cast (character IDs in use in the document);

the world canon (location, term, and entity IDs);

the asset catalog (asset IDs).

A reference should resolve within the scope appropriate to the reference type.

For example:

`char` references resolve in the cast;

`ref` on a world entity reference resolves in the corresponding world section;

`src` resolves in the asset catalog or resource system.

## 14. Forward References

OVML references are declarative.

An entity may be referenced before its declaration appears in the document.

For example, `<cast>` normally precedes `<script>`, but a conforming implementation SHOULD not require entities to be declared before every use.

The document is interpreted as a whole.

A validator MAY resolve references across the entire document rather than requiring declaration order.

## 15. Validation

An OVML validator SHOULD verify:

identifiers are present where required;

identifiers are valid within their scope;

references resolve to existing entities;

`char` values resolve to existing character IDs;

`ref` values resolve to existing world entity IDs;

`src` values are syntactically valid references.

Resource availability is a runtime concern and is not required for structural validity.

## 16. Design Principle

OVML deliberately separates:

the entity from the reference;

the reference from the physical resource;

the identifier from the display name.

This makes it possible to reuse entities, relocate resources, and maintain consistency without rewriting the creative content.

## 17. Related Documents

See: reference/world.md
See: reference/cast.md
See: reference/assets.md
See: reference/document.md
