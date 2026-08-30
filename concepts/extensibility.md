# Extensibility

**OpenVML Standard 2.2**

This document explains how OVML stays open and extensible rather than closing around a fixed set of project types or features.

OVML is designed to grow. Its extensibility comes from a small number of uniform rules, a well-defined forward-compatibility policy, and an explicit decision to leave project types and many content kinds out of the standard.

1. The Uniform Section Rule

The most important extensibility mechanism is the uniform section rule.

A world section is a container of named entities. An entity:

    has a stable id;
    has a display name;
    may contain arbitrary clarifying child elements;
    is referenced from script content by ref.

Because every section follows the same structural rule, the parser does not need to know the kind of content to interpret a section. It applies the same rule to every section.

This makes the language self-describing. New kinds of sections can be introduced without changing the parser: a lecture's glossary, a novel's locations, a documentation set's definitions all use the identical mechanism. The standard does not need to enumerate every possible section in advance.

2. The <world> Canon as the Extension Point

The <world> element applies this uniform rule at the document level. It holds the canonical entities of the project in free sections:

    world
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

<world> does not prescribe a fixed set of sections. A document declares the sections that its content requires, and whichever sections are present are parsed by the uniform rule.

This means OVML can represent recurring entities of any kind — a tavern, a term, a faction, a version — without the standard defining each one. The container is generic; the entity kinds are open.

3. Forward Compatibility

OVML deliberately preserves forward compatibility.

An implementation encountering an unknown element or attribute should not reject an otherwise valid OVML document solely because of that unknown element or attribute.

For example, a future version may introduce a new element or attribute. An OVML 2.2 implementation that does not understand it should ignore it rather than treating it as invalid or as a playback instruction.

This applies to metadata as well. OpenVML 2.2 implementations that do not understand a future metadata element should ignore it rather than treating it as a playback instruction. This is how the language grows without breaking existing documents. Validation of the document's defined OVML 2.2 elements and attributes remains the responsibility of the validator.

4. Strict Parsers Warn on Unknowns

A strict OVML parser validates the elements and attributes that the current standard defines. When it encounters an unknown element or attribute, it warns rather than silently misinterpreting it.

The warning preserves forward compatibility while keeping the document's semantics clear: the unknown content is preserved but not treated as an instruction the current standard defines. This is consistent with the meta and validation policy — unknown metadata is ignored, not executed, and not used to reject a valid document.

The standard's rule here is deliberately cautious: a documented operation is part of the language; an undocumented operation is not assumed to have a standardized representation. Unknowns are honored as potential future content, not assumed to be meaningful in the current version.

5. Versioning

Versioning is part of extensibility.

The current standard is OVML 2.2, declared in the root element:

```xml
<ovml version="2.2" lang="en">
```

Implementations use the declared version to determine which vocabulary and semantics are supported. Future versions may introduce new elements, attributes, media types, timing mechanisms, camera controls, processing hints, and extension mechanisms. Backward compatibility will be considered when evolving the standard.

Processing vocabulary is versioned in the same spirit. The current audio vocabulary is eq, compressor, delay, reverb, chorus, gain, and convert; the current video vocabulary includes color, blur, sharpen, grayscale, sepia, grain, vignette, invert, glow, lens flare, fade, overlay, and convert. Additional operations may be added when their XML representation and semantics are formally established.

6. Project Types Are Not in the Standard

A deliberate decision keeps OVML extensible: the standard does not define project types.

The standard does not prescribe a list of content categories — lecture, presentation, audiobook, game, and so on are uses of the same language, not separate formats. A parser interprets a document from its structure: whichever sections are present are parsed by the uniform rule.

Applications may map the resulting structure to their own product categories, but that mapping is outside the standard. Because OVML is not bound to a fixed set of types, new kinds of audiovisual work can be expressed without a new format. The document's structure itself describes what it is.

7. Summary

OVML stays extensible through a few consistent decisions.

    The uniform section rule lets any kind of canonical entity be added without changing the parser.

```xml
The <world> canon is a generic container with free sections.

Forward compatibility preserves unknown elements and warns without rejecting documents.

Versioning (currently 2.2) lets the language grow deliberately.

Project types are deliberately absent, so the language serves any audiovisual work.
```

Each of these keeps OVML an open format that can grow through discussion and implementation experience without breaking the documents that already exist.

See: reference/world.md
See: reference/validation.md
See: concepts/README.md
