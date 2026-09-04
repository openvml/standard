# Entity

## Overview

An **Entity** is an identifiable semantic object represented in an OpenVML document.

An entity has its own identity and MAY have additional properties, representations, state, and relationships with other entities.

Entity is a conceptual model type. It does not necessarily correspond to a literal `<entity>` element in an OpenVML document.

Concrete OpenVML elements such as `character` and `location` define specific entity types.

Additional entity types MAY be introduced by future versions of the Standard or by extensions.

## Entity Identity

Every entity MUST have a unique identifier within the scope in which the entity is defined.

For `character`, the identity is established by the `id` attribute (xs:ID) and the `name` attribute:

```xml
<character id="anna" name="Anna" gender="female" age="adult" role="main"/>
```

For `location`, the identity is established by the `id` attribute (xs:ID) and the `name` attribute:

```xml
<location id="station" name="Central Station" type="interior"/>
```

The `id` identifies the entity and MAY be referenced by other parts of the document.

Identity and representation are separate concepts.

An entity MAY have an identity without having an avatar or other visual representation.

## Entity Types

OpenVML defines entity types according to their semantic role.

The current entity types are:

* `character` — a person, creature, or other acting entity that can participate in narrative events or dialogue;
* `location` — a place or spatial context within the modeled world (defined in `<world><locations>`).

## Common Entity Properties

Entity types MAY support common semantic properties including:

* `identity` (via `id` and `name` attributes);
* references to other entities;
* metadata;
* extensions.

Not every entity type is required to support every property.

The semantics of a property are defined by the corresponding entity specification.

## Identity

Identity describes how an entity is identified within the document and by users.

For `character`:

```xml
<character id="anna" name="Anna" gender="female" age="adult" role="main"/>
```

For `location`:

```xml
<location id="station" name="Central Station" type="interior"/>
```

Identity does not describe the physical appearance or current state of the entity.

## Avatar

**Note:** The current OVML 2.2 XSD does not define an `avatar` element. Avatar semantics are documented in `avatar.md` for future consideration and application-level usage. An avatar is a visual representation used primarily for identification in user interfaces and other non-scene contexts.

Example of application-level avatar usage (not part of OVML 2.2 schema):

```xml
<character id="anna" name="Anna">
    <!-- Application-specific avatar reference -->
    <avatar asset="anna-avatar"/>
</character>
```

An avatar does not define the physical appearance of an entity and MUST NOT be interpreted as a scene instruction.

## Type-Specific Properties

Properties that describe the nature or behavior of an entity belong to the corresponding entity type.

For `character` (see `character.md`):

* `appearance_detail` — structured visual traits
* `inventory` — items the character possesses
* `condition` — physical conditions (wounds, etc.)
* `state` — per-scene physical state (posture, movement, activity)
* voice configuration (voiceId, voiceEngine, pitch, rate, timbre)
* processing preset references

For `location` (see `location.md`):

* `era`, `style`, `palette`, `props`, `atmosphere` — environment descriptors

The Standard MUST NOT assume that all entity types share the same physical or behavioral model.

## Entity and Scene

An entity may participate in a scene when its entity type supports scene participation.

Scene participation does not change the identity of the entity.

The same entity MAY participate in multiple scenes while retaining the same identity.

Scene-specific information such as position, orientation, blocking, activity, or current state MUST be represented separately from persistent identity.

For `character`, scene participation is expressed via `<characters><char ref="..." state="..."/>` or `<blocking><character ref="..." state="..."/>`. The `state` attribute uses semicolon-separated key=value pairs (posture, movement, activity, transport).

For example:

```text
Entity (character)
    id = anna

Scene A
    state = posture=standing;movement=stationary;activity=observing

Scene B
    state = posture=sitting;movement=stationary;activity=reading
```

The entity remains the same entity in both scenes.

## Entity and Representation

An entity is a semantic object, not a rendered object.

A renderer MAY represent an entity using:

* an avatar (application-level);
* a character model;
* a sprite;
* an image;
* a video;
* a 3D model;
* text;
* an icon;
* another supported representation.

The OpenVML document describes the semantic entity and its state.

The consuming application determines how that entity is rendered.

## Entity Relationships

Entities MAY reference other entities.

For example:

```xml
<character id="anna" name="Anna"/>
<location id="station" name="Central Station"/>
```

A scene may establish relationships between them:

```text
anna
    interacts-with
    (via blocking/character references in scene)

anna
    located-at
    station
    (via <location ref="station"/> in scene)
```

Relationship semantics are defined by the corresponding OpenVML constructs.

Entity references SHOULD use stable entity identifiers rather than duplicating the entity definition.

## Entity Lifetime

An entity definition establishes the identity of the entity.

An entity MAY appear in multiple chapters or scenes.

Scene participation, visibility, activity, or temporary state does not create a new entity.

A new entity MUST have a distinct identifier.

## Conceptual Model

The OpenVML entity model can be represented as:

```text
Entity
│
├── Identity (id, name)
├── Type-specific properties
│   ├── Character (appearance_detail, inventory, condition, voice, state)
│   └── Location (era, style, palette, props, atmosphere)
│
└── Scene participation
    ├── State
    ├── Position
    ├── Orientation
    └── Relationships
```

This model separates three different concerns:

```text
WHO / WHAT
    Entity + Identity (id, name)

HOW IT IS REPRESENTED
    Avatar (application-level) + Appearance_detail + Media

WHAT IS HAPPENING NOW
    State + Scene State + Relationships
```

This separation is fundamental to the OpenVML model.

> **An entity is the thing being modeled.
> Its representation is how an application shows it.
> Its state describes what it is doing or how it currently exists.**
