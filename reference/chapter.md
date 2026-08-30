> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Chapter — `<chapter>`

**OVML Standard 2.2**

## 1. Purpose

The `<chapter>` element is a logical unit of the script.

A chapter is a major navigable section of an OVML project.

Examples include:

- a book chapter;
- a lesson;
- a presentation section;
- an episode segment;
- a game sequence;
- a film sequence;
- a podcast segment.

Chapters may be used for:

- navigation;
- displaying project structure;
- navigating between sections;
- organizing long-form projects;
- defining logical boundaries in the script.

## 2. Structure

A `<chapter>` is a child of the `<script>` element and contains scenes and content blocks.

Example:

<script>

```
<chapter id="chapter-1" title="Beginning">

    <scene>
        ...
    </scene>

</chapter>

<chapter id="chapter-2" title="The Meeting">

    <scene>
        ...
    </scene>

</chapter>
```

</script>

Conceptually:

<script>
    │
```
└── <chapter>
    │
    ├── <scene>
    │   ├── <camera>
    │   ├── text
    │   ├── media
    │   └── ...
    │
    └── <scene>
```

A script MAY contain one or more chapters.

Different project types may contain different numbers of chapters and scenes.

## 3. Attributes

The following chapter attributes are commonly used.

Attribute	Type	Required	Description
id	string	No	Unique chapter identifier
title	string	No	Display title of the chapter

Example:

<chapter id="chapter-01" title="Arrival">
    ...
</chapter>

The exact metadata model for chapters is implementation-dependent unless explicitly defined elsewhere in the OVML specification.

An implementation MAY associate a chapter with:

an identifier;
a title;
an index;
a thumbnail;
duration;
metadata;
navigation information.

If chapter attributes are used, implementations SHOULD preserve unknown attributes when reading and writing OVML documents.

Implementations MAY also support additional attributes such as order_index, status, or resolution when their workflows require them.

Example:

<chapter
```
id="ch_01"
title="Chapter 1: The Beginning"
order_index="0"
status="draft">

<description>Introduction of the main character</description>

<scene>
    ...
</scene>
```

</chapter>

## 4. Chapters and Scenes

A chapter MAY contain multiple `<scene>` elements.

A scene represents a continuous logical audiovisual context.

<chapter>

```
<scene color="#1a1a2e" atmosphere="quiet evening">

    ...

</scene>

<scene color="#3b1f1f" atmosphere="danger and tension">

    ...

</scene>
```

</chapter>

Chapters provide the outer structure; scenes provide the internal dramatic and visual boundaries.

See: reference/scene.md

## 5. Short Projects

A short project may contain only one chapter.

A script without chapters MAY be supported by implementations for simple projects.

Portable OVML documents SHOULD use `<chapter>` elements when the content has meaningful navigational sections.

The simplest valid project can contain a single chapter:

<script>

```
<chapter title="Greeting">

    <scene>

        <line>
            Hello, world!
        </line>

    </scene>

</chapter>
```

</script>

## 6. Chapter Boundaries and Navigation

The chapter boundary provides a natural navigation point for Players.

A Player MAY expose chapters through:

chapter navigation;
progress indicators;
bookmarks;
chapter lists;
accessibility controls.

Navigation MAY allow the user to:

move to the next chapter;
move to the previous chapter;
select a chapter;
resume from a chapter;
display chapter progress.

This is useful for:

- audiobook chapter navigation;
- lesson and course structure;
- presentation sections;
- episode-based content;
- long-form narrative projects.

OVML does not require a specific user-interface representation.

## 7. Chapters and the Document Canvas

The project-level document model describes <chapter> as the container between <script> and <scene>.

See: reference/document.md

## 8. Validation

An OVML validator SHOULD verify:

- `<chapter>` elements are properly nested inside `<script>`;
- `id`, when present, is a valid identifier;
- `title`, when present, is plain text;
- child elements are allowed in a chapter context;
- scenes and blocks are correctly enclosed within the chapter.

The exact structure of a chapter is defined by the OVML specification and schema.

## 9. Design Principle

A chapter provides high-level logical organization.

It does not prescribe rendering or playback behavior.

The chapter boundary is semantic: it describes a navigable section of the project, while the Player determines how that section is presented and navigated.

## 10. Related Documents

See: reference/document.md
See: reference/body.md
See: reference/scene.md