> **OpenVML — Open Voice Markup Language**
> 
> An open, declarative format for describing voice-driven audiovisual content, including dialogue,
> narration, scenes, media, timing, and synchronization.

# Validation and Conformance

**OVML Standard 2.2**

## 1. Purpose

This document describes how an OVML document is validated and what conformance means for an
implementation.

An OVML validator checks the structural and syntactic validity of an OVML document.

It does not determine the artistic quality, semantic desirability, or runtime behavior of the
document.

Validation is distinct from interpretation and from playback.

## 2. What a Validator Checks

An OVML validator SHOULD verify structural and syntactic correctness.

A validator may check:

elements are properly opened and closed;

attributes have valid names;

required attributes are present;

enumeration attributes contain allowed values;

boolean attributes contain valid boolean values;

numeric attributes contain valid numbers;

color attributes use the required hexadecimal format;

identifiers are present and unique within their scope;

references resolve to existing entities;

child elements are allowed in the given context.

For example, a validator might verify:

```xml
<scene id="ch1" time="evening" mood="dramatic">
    ...
</scene>
```

where:

time contains an allowed value;

mood contains an allowed value;

the scene element is properly structured.

## 3. Validation of Media

For media and timing elements, a validator MAY verify:

src is present;

numeric attributes contain valid numbers;

layer contains an allowed value;

startMode contains an allowed value;

volume is within the supported range;

grid attributes contain valid integers;

sizePercent contains a valid number;

trimStart and duration are not negative.

See: [`reference/media.md`](media.md)
See: [`reference/timing.md`](timing.md)

## 4. Validation of References

A validator SHOULD verify the correctness of references according to the available project context.

character/@id resolves references in script content;

world entity ref values resolve to existing entity IDs;

asset references use valid reference syntax.

For external URLs, a validator is not required to verify that the resource is currently reachable.

Resource availability is a runtime concern.

See: [`reference/identifiers.md`](identifiers.md)

## 5. Structural Validity vs. Semantic Interpretation

Conformance distinguishes structural validity from semantic interpretation.

Structural validity

Whether the document follows the OVML XML structure and allowed values.

Semantic interpretation

What a conforming Player should understand from the document.

A validator is responsible for structural and syntactic validity.

Semantic interpretation belongs to the conforming Player of the document.

For example, a validator determines whether an emotion value is an allowed enumeration value.

It does not determine whether the rendering of that emotion is correct or appropriate.

## 6. Semantic Interpretation vs. Runtime Behavior

Runtime behavior is a further concern separate from both validation and interpretation.

Runtime behavior

How a particular Player implements playback, buffering, decoding, synthesis, rendering, or other
platform-specific operations.

Validation and interpretation do not predict runtime behavior.

For example, the validator does not determine:

whether a TTS provider is available;

whether a voice exists at runtime;

whether an API key is valid;

whether a media codec is supported;

whether a remote server is available;

whether a resource requires authentication;

whether a Player can decode a resource;

how overlapping streams are synchronized.

These are runtime or implementation concerns.

## 7. The Validator Is Not an Artistic Critic

A validator does not determine whether a creative decision is desirable.

For example, it should not attempt to determine:

"whether a sad scene should really be blue."

nor whether overlapping speech, video, audio, and images are good design.

Overlapping speech, video, audio, and images can be completely valid OVML behavior when their timing
relationships are correctly specified.

## 8. XML and Whitespace

An OVML document is an XML document.

Validation includes well-formedness as required by the XML language.

Whitespace, entity escaping, and attribute quoting follow XML conventions.

## 9. XSD Reference

A simplified XSD describes the OVML structure.

The canonical schema reference is:

schema/ovml-2.2.xsd

The simplified XSD defines the root element and its main children.

The root element:

```xml
<xs:element name="ovml">
    <xs:complexType>
        <xs:sequence>
            <xs:element ref="meta" minOccurs="0"/>
            <xs:element ref="settings" minOccurs="0"/>
            <xs:element ref="cast" minOccurs="0"/>
            <xs:element ref="assets" minOccurs="0"/>
            <xs:element ref="script" minOccurs="0"/>
        </xs:sequence>
        <xs:attribute name="version" type="xs:string"/>
        <xs:attribute name="lang" type="xs:language"/>
    </xs:complexType>
</xs:element>
```

The simplified XSD defines elements such as:

meta;

character;

p;

dialogue;

narrator;

w;

audio;

img;

break.

The character element in the simplified XSD:

```xml
<xs:element name="character">
    <xs:complexType>
        <xs:sequence>
            <xs:element ref="voice" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="voice_params" minOccurs="0"/>
        </xs:sequence>
        <xs:attribute name="id" type="xs:ID" use="required"/>
        <xs:attribute name="name" type="xs:string"/>
        <xs:attribute name="gender" type="xs:string"/>
        <xs:attribute name="age" type="xs:string"/>
        <xs:attribute name="role" type="xs:string"/>
        <xs:attribute name="color" type="xs:string"/>
    </xs:complexType>
</xs:element>
```

The simplified XSD is informational. The full OVML 2.2 conformance rules are described by the
individual reference documents and the normative specification.

## 10. Forward Compatibility

Implementations should preserve forward compatibility.

An implementation encountering an unknown element or attribute should not reject an otherwise valid
OVML document solely because of that unknown element or attribute.

For example, a future version may introduce a new element or attribute.

An OVML 2.2 implementation that does not understand it should ignore it rather than treating it as
invalid or as a playback instruction.

Validation of the document's defined OVML 2.2 elements and attributes remains the responsibility of
the OVML validator.

This allows the language to grow without breaking existing documents.

## 11. Runtime Duration Is Not a Validation Problem

Some durations are only known at runtime.

For example, a dynamically synthesized line:

```xml
<line char="narrator">
    Dynamically synthesized speech.
</line>
```

The final audio duration depends on the TTS engine and generated audio.

The Player may need to resolve sequential timing dynamically.

This is not a validation problem.

## 12. Conformance Summary

An implementation should distinguish three concepts:

Structural validity

The document is well-formed and uses allowed values.

Semantic interpretation

A conforming Player understands the document's meaning.

Runtime behavior

A particular Player executes the document on a platform.

Validation addresses the first concern.

The standard describes the second.

The Player owns the third.

## 13. Related Documents

See: [`concepts/scenes-and-world.md`](../concepts/scenes-and-world.md)
See: [`reference/identifiers.md`](identifiers.md)
See: [`reference/media.md`](media.md)
See: [`reference/timing.md`](timing.md)
See: [`reference/emotions.md`](emotions.md)
See: [`reference/README.md`](README.md)
