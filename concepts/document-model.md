# OpenVML Document Model

**OpenVML Standard 2.2**

This document explains the conceptual model of an OVML document: how the top-level structure is
organized, what each section is for, and why the document is best understood as a container of
named, canonical entities that reference each other by stable ids.

An OVML document is a declarative audiovisual project script. It describes the project's content,
characters, resources, timing, and directorial instructions. It does not define a specific playback
or rendering technology.

## 1. The Document as a Container

Every OVML document is a single XML tree rooted at the `<ovml>` element:

```xml
    <ovml version="2.2" lang="en">
        ...
    </ovml>
```

The root declares the standard version and the primary language of the document.

Conceptually, the document is not a flat list of media files. It is a container of named canonical
entities — characters, assets, world entities, scenes, chapters — each of which is declared in one
place with a stable id. The rest of the document references those entities by id instead of
repeating their definitions.

This is the key idea: an entity is declared once, and every use of it is a reference. Reusing an
entity, relocating a resource, or renaming a display string does not require rewriting the creative
content that uses it.

## 2. The Top-Level Sections

The root element contains several major sections, each with a distinct responsibility:

```xml
    <ovml version="2.2" lang="en">

        <meta>
            ...
        </meta>

        <settings>
            ...
        </settings>

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
```

The primary sections are:

```xml
    <meta> — project metadata and presentation preferences;
    <settings> — general document-level settings;
    <cast> — characters, voices, and character-related processing;
    <assets> — media resource definitions;
    <world> — the canonical entities of the project's world;
    <script> — the main content and timeline structure.
```

Not every section is required. A minimal document can contain only the root and a script, and the
simplest content may be nothing more than a title and a few lines.

3. `<meta>` — Project Metadata

The `<meta>` element contains information about the project itself: its title, author, and general
presentation preferences such as whether subtitles are shown by default and how they are styled.

Metadata identifies the project but does not define its content, timeline, media, voices, or
rendering. It is descriptive, not behavioral. The Player may override presentation preferences with
user-level settings.

As with every section, unknown future metadata is preserved rather than treated as a playback
instruction.

1. [`reference/meta.md`](../reference/meta.md)

4. `<cast>` — Characters

The `<cast>` element defines the characters used by the project. Each character is a canonical entity
with a stable id and a human-readable name, and may carry identity information, aliases, descriptive
text, voice configuration, and optional processing preset references.

The script references a character by id through the char attribute of a line:

```xml
    <line char="alex">
        We have to go.
    </line>
```

The character is declared once; the line only refers to it. Changing the character's voice or
description does not change any line that references it.

1. [`reference/cast.md`](../reference/cast.md)
2. [`concepts/characters-and-voices.md`](characters-and-voices.md)

5. `<assets>` — Resources

The `<assets>` element describes the resources used by the project. A resource may be an image, video,
audio, music track, or sound effect. Assets may be referenced externally, by logical id, or as
packaged resources inside an OVMZ container.

The script references an asset through the src attribute of a media element:

```xml
    <video src="background-video" />
```

The asset is the resource; the media element is an instruction for using that resource at a
particular point in the timeline.

1. [`reference/assets.md`](../reference/assets.md)
2. [`concepts/media-layers.md`](media-layers.md)

6. `<world>` — The World Canon

The `<world>` element declares the canonical entities of the project's world: locations, terms,
factions, timelines, or any other section the project requires. It uses the same mechanism
everywhere: a section is a container of named entities, each entity has a stable id and a name, and
a scene references an entity by ref instead of duplicating its description.

```xml
    <world>
        <locations>
            <location
                id="rusty_anchor"
                name="The Rusty Anchor">
                ...
            </location>
        </locations>
    </world>
```

A scene references the canonical entity and expresses scene-specific change with a `<variation>`:

```xml
    <scene>
        <location ref="rusty_anchor">
            <variation>
                <weather>rainy</weather>
            </variation>
        </location>
    </scene>
```

The world keeps recurring entities consistent across many scenes and chapters. It is optional: a
document without a world canon is valid.

1. [`reference/world.md`](../reference/world.md)
2. [`concepts/scenes-and-world.md`](scenes-and-world.md)

7. `<script>` — The Content

The `<script>` element contains the actual content of the work. It organizes content into a hierarchy:

```text
    script
    │
    └── chapter
        │
        └── scene
            │
            ├── camera
            ├── p
            │   └── line
            │       └── w
            ├── img
            ├── video
            ├── audio
            └── break
```

    a chapter is a logical, navigable unit of the script;
    a scene groups content that belongs to the same creative context;
a block is an element such as a line, an image, a video, an audio clip, a pause, or a camera
instruction;
    a line may contain word-level markup.

The exact set of allowed children is defined by the individual element specifications.

## 8. Stable Ids and Refs

All of the document's named entities share one model:

```xml
    <meta> tags, characters, assets, world entities, scenes, and chapters are identified by stable ids;
    the places that use them carry a reference.
```

    "character" is referenced by char;
    "asset" is referenced by src;
    "world entity" is referenced by ref;
    "camera target" is referenced by target.

This uniform rule is what makes OVML self-describing and extensible. A parser reads the structure of
the document and applies the same pattern to every section, without needing to know a special
project type.

## 9. Minimal and Complete Documents

The conceptual hierarchy is not a checklist. Every element is optional where its section says so,
and many documents are intentionally small.

The smallest useful document contains only the root and a script with a single chapter, scene, and
line:

```xml
    <ovml version="2.2" lang="en">

        <script>

            <chapter title="Greeting">

                <scene>

                    <line>
                        Hello, world!
                    </line>

                </scene>

            </chapter>

        </script>

    </ovml>
```

For further reading on the complete hierarchy and its many examples, consult the reference.

1. [`reference/document.md`](../reference/document.md)
2. [`reference/meta.md`](../reference/meta.md)
3. [`reference/README.md`](../reference/README.md)
