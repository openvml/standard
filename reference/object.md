> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# `<object>` — Objects

**OVML Standard 2.2**

## 1. Purpose

The `<object>` element defines a distinct thing or resource that may exist independently within the modeled world. An object is an **Entity type** representing inanimate items, props, devices, or resources that characters may interact with.

**Status:** The `<object>` element is **not yet defined in the OVML 2.2 XSD**. This document describes the planned entity model for future standardization. Applications MAY implement object support as an extension.

Objects are conceptually declared at the document level (location TBD — likely under `<world>` or a new top-level container).

## 2. Conceptual Structure

The general conceptual structure is:

```xml
<!-- Not yet in OVML 2.2 XSD -->
<object
    id="radio"
    name="Old Radio"
    type="device">

    <appearance_detail>
        <material>wood, metal</material>
        <condition>worn, dusty</condition>
    </appearance_detail>

    <condition>
        <item id="crack" state="active" description="Cracked speaker grill"/>
    </condition>

    <state posture="stationary" activity="playing"/>

</object>
```

**Note:** In OVML 2.2, identity is established by `id` and `name` attributes directly on the element (same as `character` and `location`), not by a separate `<identity>` child element.

Each `<object>` MUST have a unique `id` within the OVML document.

## 3. Identity

Object identity is established by the `id` and `name` attributes (planned), following the same pattern as `character` and `location` — attributes directly on the element, not a separate `<identity>` child element.

**Note:** The current OVML 2.2 schema does not define an `<object>` element. When added, it should use attribute-based identity consistent with existing entity types.

## 4. Object as Entity

An object is an **Entity type** with the following conceptual structure:

```text
Object
├── Identity (id, name)
├── Avatar (application-level, future extension)
├── Appearance_detail (material, condition, visual traits)
├── Condition (damage, wear, functional state)
├── State (posture, activity — for animate/automated objects)
└── Scene participation (via blocking or scene references)
```

## 5. Type-Specific Properties (Planned)

### Appearance_detail

Structured visual traits for media generation:

```xml
<appearance_detail>
    <material>wood, brushed metal</material>
    <color>dark walnut, copper accents</color>
    <form>rectangular, vintage tabletop radio</form>
    <size>30cm × 18cm × 15cm</size>
</appearance_detail>
```

### Condition

Physical/functional conditions:

```xml
<condition>
    <item id="speaker_crack" state="active" description="Cracked speaker grill, audio distorted"/>
    <item id="dial_wear" state="healed" description="Worn frequency dial, restored"/>
</condition>
```

Allowed `state` values: `active`, `healing`, `healed`.

### State

Per-scene physical state (for objects that can move or change pose):

```xml
<state posture="stationary" activity="playing" movement="stationary"/>
```

Keys: `posture`, `movement`, `activity`, `transport` (for vehicles).

## 6. Scene Participation (Planned)

Objects MAY participate in scenes through:

- `<blocking>` — semantic positioning and relationships
- Scene references — objects present in a scene

Example (conceptual):

```xml
<scene>
    <blocking>
        <character ref="anna" position="left" look_at="radio"/>
        <object ref="radio" position="right" state="posture=stationary;activity=playing"/>
    </blocking>
</scene>
```

**Note:** The current OVML 2.2 XSD does not support `<object>` in `<blocking>`. This is planned for future versions.

## 7. Relationships

Objects MAY be referenced by characters and locations:

```text
anna
    interacts-with
radio

radio
    located-at
station
```

## 8. Object vs Character vs Location

| Aspect | Character | Object | Location |
|--------|-----------|--------|----------|
| Agency | Acts, speaks | Passive (usually) | Spatial context |
| Voice | Yes (voice config) | No | No |
| Inventory | Yes | No | No |
| Condition | Yes | Yes (planned) | No |
| Appearance_detail | Yes | Yes (planned) | Via style/palette |
| Scene State | Yes | Yes (planned) | Via variation |

## 9. Validation (Future)

When `<object>` is added to the schema, a validator SHOULD verify:

- `object/@id` is present and unique;
- `object/@name` is present;
- `condition/item/@state` contains allowed values: `active`, `healing`, `healed`;
- `state` attribute parses as semicolon-separated key=value pairs.

## 10. Design Principle

An object defines **what** exists in the world as a distinct, referenceable thing. It separates the semantic entity from its representation and state, consistent with the Entity model.

> **Model the thing once. Let each scene choose its moment.**