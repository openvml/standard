> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# `<location>` — Locations

**OVML Standard 2.2**

## 1. Purpose

The `<location>` element defines a place or spatial context within the modeled world. A location is an **Entity type** representing a semantic spatial entity that can be referenced by scenes and world canon.

Locations are declared inside the `<world><locations>` element.

The `<world>` element is optional for projects that do not require a world canon.

1. [`reference/world.md`](world.md)
2. [`reference/scene.md`](scene.md)

## 2. Structure

The general structure is:

```xml
    <world>
        <locations>
            <location
                id="station"
                name="Central Station"
                type="interior"
                era="modern"
                style="industrial"
                palette="steel, concrete, neon"
                props="turnstiles, platforms, information boards"
                atmosphere="busy, echoing, cold"/>
        </locations>
    </world>
```

Each `<location>` MUST have a unique `id` within the OVML document.

## 3. Required Attributes

The following attributes are required:

| Attribute | Type | Description |
|-----------|------|-------------|
| `id` | ID | Unique location identifier |
| `name` | string | Human-readable location name |

Example:

```xml
    <location
        id="station"
        name="Central Station" />
```

The `id` is used to reference the location from scenes and other constructs.

## 4. Optional Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| `type` | string | Location type classification (e.g., `interior`, `exterior`, `virtual`) |

Example:

```xml
    <location
        id="station"
        name="Central Station"
        type="interior" />
```

## 5. Child Elements

The `<location>` element MAY contain the following child elements (all optional):

| Element | Type | Description |
|---------|------|-------------|
| `era` | string | Historical period or time setting (e.g., `modern`, `medieval`, `futuristic`) |
| `style` | string | Architectural or visual style (e.g., `industrial`, `victorian`, `minimalist`) |
| `palette` | string | Color palette description (e.g., `steel, concrete, neon`) |
| `props` | string | Notable props or features (e.g., `turnstiles, platforms`) |
| `atmosphere` | string | Mood or atmospheric description (e.g., `busy, echoing, cold`) |

Example with all child elements:

```xml
    <location
        id="station"
        name="Central Station"
        type="interior">

        <era>modern</era>
        <style>industrial</style>
        <palette>steel, concrete, neon</palette>
        <props>turnstiles, platforms, information boards</props>
        <atmosphere>busy, echoing, cold</atmosphere>

    </location>
```

## 6. Location as Entity

A location is an **Entity type** with the following conceptual structure:

```text
Location
├── Identity (id, name)
├── Avatar (application-level, future extension)
├── Environment / Spatial properties (era, style, palette, props, atmosphere)
├── State (if applicable)
└── Relationships (factions, connections to other locations)
```

Location identity is established by the `id` and `name` attributes. Location does not have a separate `<identity>` child element — identity is carried by attributes.

**Avatar** is not currently part of the OVML 2.2 schema for location. Application-level avatar support is documented in `avatar.md`.

**Appearance** is not a separate element for location. Visual characteristics are described through `style`, `palette`, `props`, and `atmosphere`.

## 7. Scene Participation

A scene references a canonical location from the project's world canon through a child `<location>` element with a `ref` attribute:

```xml
<scene id="ch1_arrival" time="evening" mood="tense">

    <location ref="station">
        <variation>
            <weather>rainy</weather>
        </variation>
    </location>

    <characters>
        <char ref="anna" state="posture=standing;movement=walking;activity=operating"/>
    </characters>

</scene>
```

The `ref` attribute MUST correspond to an existing `location/@id` in `<world><locations>`.

The `<variation>` child allows scene-specific overrides of the location's environment (time of day, weather).

## 8. Location vs Scene

A **location** is an Entity of the world — a persistent place that exists independently of any particular scene.

A **scene** is a narrative/rendering context — a specific moment or segment that takes place at a location.

They are distinct concepts:

| Concept | Purpose | Example |
|---------|---------|---------|
| Location | Semantic spatial entity in the world | "Central Station" |
| Scene | Narrative segment at a location | "Chapter 1: Arrival at the station" |

A single location MAY be used by multiple scenes. A scene MAY reference a location without being synonymous with it.

## 9. Relationships

Locations MAY participate in relationships defined in the world canon:

```xml
<world>
    <locations>
        <location id="station" name="Central Station"/>
        <location id="plaza" name="Station Plaza"/>
    </locations>
    <factions>
        <!-- factions may control or be associated with locations -->
    </factions>
</world>
```

Scene blocking MAY establish spatial relationships between characters and locations implicitly through the scene's location reference.

## 10. Validation

An OVML validator SHOULD verify:

- `location/@id` is present and unique within `<world><locations>`;
- `location/@name` is present;
- `location` references in scenes (`<location ref="...">`) resolve to existing location IDs;
- child elements (`era`, `style`, `palette`, `props`, `atmosphere`) contain valid string values.

The validator checks structural validity. It does not determine the artistic quality of the location description.

## 11. Design Principle

A location defines **where** things happen in the modeled world. It is a semantic entity, not a rendering instruction.

The same location can serve as the backdrop for many different scenes, each with its own variation (time, weather, atmosphere). This separation allows the world to be modeled once while enabling rich narrative variation.

> **Model the place once. Let each scene choose its moment.**