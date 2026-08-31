# Cross-References

**OpenVML Standard 2.2**

This document explains the id and reference system that runs through the whole OVML document,
connecting characters, scenes, chapters, locations, assets, and camera targets to each other.

OVML is built on a simple idea: entities are declared once with a stable id, and every other place
that needs the entity refers to it by reference. This keeps long-form projects consistent and makes
the whole document navigable.

## 1. Identifiers and References

An identifier is a key that names a reusable entity. A reference is an attribute that points back to
that identifier from another place in the document.

For example, a character is declared in <cast>:

```xml
    <character id="alex" name="Alex" />
```

The script then references that character through the char attribute:

```xml
    <line char="alex">
        Welcome.
    </line>
```

The same pattern recurs for every kind of entity in the document.

## 2. What Carries an Id

Entities that are referenced by other parts of a document carry a stable id:

    character id;
    scene id;
    chapter id;
    location id;
    asset id;
    entity id in a world section.

An identifier should remain stable within the scope in which it is referenced. Changing the physical
resource or the display name of an entity does not necessarily change its identifier.

## 3. How References Point

Each kind of entity is referenced through a specific attribute.

    "character" is referenced by the char attribute of a line.

    "asset" is referenced by the src attribute of a media element.

    "world entity" is referenced by the ref attribute of a scene child element.

    "camera target" is referenced by the target attribute of a camera.

a scene or chapter may be referenced by its id from an addressable object defined by the
implementation.

These references are the edges of the document's graph. They connect the places where the entity is
used to the single place where it is declared.

## 4. Referencing Assets

Assets are referenced through src:

```xml
    <video src="forest-video" />
```

The value may be:

    a logical asset id;
    an external URL;
    a package-local path;
    another resource identifier supported by the implementation.

A project SHOULD prefer logical asset identifiers when it has an asset catalog, because this lets
the resource location change without changing the script. The asset identity remains stable within
the scope in which it is referenced.

## 5. Referencing World Entities

A scene references a canonical world entity through a child element with a ref attribute:

```xml
    <scene>
        <location ref="rusty_anchor">
            <variation>
                <weather>rainy</weather>
            </variation>
        </location>
    </scene>
```

The ref value is the id of an entity declared in the corresponding section of <world>. The mechanism
is identical for every section — a location, a term, a faction all follow the same rule.

```xml
Some references permit a plain-text form without ref for backward compatibility; for example, <location>A dark forest.</location> remains valid.
```

## 6. Referencing the Camera Target

The camera target identifies the intended subject of the camera:

```xml
    <camera
        shot="close"
        framing="center"
        target="alex" />
```

The target may refer to a character id, a visual object, an element id, or another addressable
object defined by the implementation. For a character, the value should correspond to the
character's id in <cast>. The exact target namespace depends on the project structure.

## 7. Resolution Scope

A reference resolves within the scope appropriate to its type.

    char references resolve in the cast;

    ref on a world entity reference resolves in the corresponding world section;

    src resolves in the asset catalog or resource system.

An identifier must be unique within its resolution scope. For example, character ids must be unique
within the document, location ids within the world canon, asset ids within the asset catalog. Two
entities in different scopes may use the same identifier value where their resolution scopes do not
overlap. This is why a character id and a world location id may share a value without conflict.

## 8. Forward References

OVML references are declarative. An entity may be referenced before its declaration appears in the
document.

For example, <cast> normally precedes <script>, but a conforming implementation should not require
every entity to be declared before every use. The document is interpreted as a whole, and a
validator may resolve references across the entire document rather than requiring declaration order.

## 9. Why the System Matters

The id and reference system is what keeps the document manageable.

Entities are declared once and reused many times, so there is no drift between duplicated
descriptions.

References separate the entity from the instruction that uses it, the reference from the physical
resource, and the identifier from the display name.

Resources can be relocated, characters re-voiced, and names changed without rewriting the creative
content that uses them.

This is the mechanism that lets scenes reference locations, lines reference characters, media
reference assets, and cameras reference targets — the cross-references that bind an OVML project
together.

See: [`reference/identifiers.md`](../reference/identifiers.md)
See: [`reference/world.md`](../reference/world.md)
See: [`reference/cast.md`](../reference/cast.md)
