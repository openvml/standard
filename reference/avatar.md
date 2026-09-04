# Avatar

## Overview

An **avatar** is an optional visual representation of an entity used primarily for identification in user interfaces and other non-scene contexts.

An avatar represents **the identity of an entity**, but does not define its physical appearance or state within a scene.

The avatar is therefore distinct from:

* `appearance_detail`, which describes the visual characteristics of an entity;
* `thumbnail`, which provides a preview representation of a project, scene, or other content;
* scene state, which describes how an entity is positioned and represented within a scene.

An avatar may be used by players, editors, authoring tools, character selectors, credits, catalogs, publishing systems, or other interfaces.

**Note:** The current OVML 2.2 XSD does not define an `avatar` element. This document describes the semantic concept for application-level usage and future standardization.

## Semantics

An avatar is a selected representation of an entity for identification.

The existence of an avatar does not imply that the represented entity looks like the avatar inside a scene.

For example, a character may have a portrait used as an avatar while having a completely different appearance, costume, pose, or visual representation during a scene.

This distinction allows an OpenVML document to describe the entity once while allowing different applications to choose how that entity is presented.

The conceptual relationship (for `character` entity type):

```text
Character
├── Identity (id, name)
├── Avatar (application-level)
├── Appearance_detail
├── Inventory
├── Condition
├── State
└── Scene State
```

Other entity types (`location`, future `object`) have different type-specific properties (see `entity.md`).

`Avatar` is semantically associated with the identity of an entity rather than with its physical state.

## Syntax (Conceptual)

An avatar is conceptually declared using the `avatar` element and references an asset:

```xml
<character id="anna" name="Anna">
    <avatar asset="anna-avatar"/>
    <appearance_detail>
        ...
    </appearance_detail>
</character>
```

The referenced asset is declared in the document's asset collection:

```xml
<assets>
    <image
        id="anna-avatar"
        src="resources/characters/anna-avatar.png"/>
</assets>
```

The `asset` attribute identifies the resource used as the avatar.

**Note:** In OVML 2.2, this syntax is not part of the normative schema. Applications MAY implement avatar support as an extension.

## Optionality

An avatar is optional.

An entity MAY have no avatar.

OpenVML does not require a player or authoring application to generate a default avatar when one is not provided.

Applications MAY provide their own fallback representation, such as:

* a generic placeholder;
* an automatically generated portrait;
* an icon derived from the entity identity;
* another application-specific representation.

Such fallback behavior is outside the OpenVML document semantics.

## Entity Types

Avatar is not inherently limited to characters.

Any entity that has an identity and may need a visual representation outside a scene MAY have an avatar.

For example:

```xml
<character id="anna">
    <avatar asset="anna-avatar"/>
</character>

<!-- Future extension: -->
<object id="old-radio">
    <avatar asset="old-radio-avatar"/>
</object>

<location id="london">
    <avatar asset="london-avatar"/>
</location>
```

The exact set of entity types supporting `avatar` is defined by the corresponding entity specifications.

## Avatar and Appearance_detail

`avatar` and `appearance_detail` MUST NOT be treated as equivalent concepts.

`appearance_detail` describes what an entity physically or visually looks like.

For example:

```xml
<appearance_detail>
    <hair color="black" length="shoulder" style="wavy"/>
    <eyes color="green"/>
</appearance_detail>
```

An avatar selects a particular visual representation:

```xml
<avatar asset="anna-portrait"/>
```

The same entity may therefore have:

```text
Identity
    Anna

Avatar
    anna-portrait.png

Appearance_detail
    black hair
    green eyes
    adult

Scene State
    standing
    facing north
```

Changing an avatar does not necessarily change the entity's appearance_detail.

Changing an entity's appearance_detail does not necessarily require changing its avatar.

## Avatar and Thumbnail

An avatar identifies an entity.

A thumbnail provides a preview of content.

For example:

```xml
<project>
    <thumbnail asset="project-cover"/>
</project>
```

and:

```xml
<character id="anna">
    <avatar asset="anna-avatar"/>
</character>
```

A thumbnail is normally associated with a project, chapter, scene, media item, or other content that needs a preview.

An avatar is normally associated with an entity that needs an identity representation.

Applications MAY use the same underlying asset for both purposes, but the semantic roles remain different.

## Representation Type

The OpenVML Standard defines the semantic role of an avatar but does not require a particular visual medium.

An avatar MAY be represented by an image or another asset type supported by the document and the consuming application.

The Standard does not require an avatar to be:

* a photographic portrait;
* a static image;
* a human face;
* a specific resolution;
* a specific aspect ratio.

Applications MAY impose additional presentation requirements for their own interfaces.

## Multiple Representations

The base `avatar` property represents the preferred or default avatar of an entity.

If an application requires multiple representations for different contexts, this SHOULD be handled by an extension or by a future representation mechanism rather than assigning multiple unrelated meanings to `avatar`.

For example, an application may eventually distinguish:

```text
avatar       — identity representation
thumbnail    — content preview
portrait     — character portrait
icon         — compact symbolic representation
```

These concepts SHOULD remain semantically distinct even when they reference the same underlying asset.

## Rendering

The OpenVML Standard does not prescribe how an avatar is rendered.

A Player, Studio, Marketplace, or other application MAY render the avatar in any suitable context.

For example:

* a character list may display the avatar next to the character name;
* a dialogue interface may display it beside a speaker;
* a marketplace may display it on a product page;
* an authoring tool may display it in a character selector;
* a player may display it in credits or metadata.

An avatar MUST NOT be interpreted as a scene instruction.

In particular, the presence of:

```xml
<avatar asset="anna-avatar"/>
```

does not instruct the renderer to place that image into the scene.

## Design Principle

OpenVML separates **entity semantics** from **application presentation**.

The document defines which representation belongs to an entity.

The consuming application decides how and where that representation is presented.

This allows the same OpenVML document to be used by different applications without requiring the document to encode UI-specific behavior.

> **Model the entity once. Let each renderer decide how to represent it.**
