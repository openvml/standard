> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# Subtitles — `<subtitle>`

**OVML Standard 2.2**

## 1. Purpose

The `<subtitle>` element represents subtitle or caption content associated with a project or with a specific media element.

Subtitles make spoken and narrative content readable.

OVML separates three related concepts:

- subtitle content — the subtitle text itself, in one or more languages;
- subtitle resources — subtitle tracks referenced by a project or a media element;
- subtitle preferences — how subtitles are displayed, declared in `<meta>`.

## 2. Subtitle Content

A `<subtitle>` element carries subtitle text.

Example:

<subtitle lang="en">
    The forest was silent.
</subtitle>

Attributes:

Attribute	Type	Required	Description
lang	language tag	No	Language of the subtitle text

The text content is preserved as authored content.

The value of lang uses language identifiers such as:

en
en-US
ru
ru-RU
uk
uk-UA

## 3. Subtitle as a Resource

A subtitle may also be declared as a resource identified by id and src.

Example:

<subtitle
`` `id="subs_ru"
type="srt"
name="Russian subtitles">
<src>gdrive:subtitles/video_ru.srt</src>
`` `</subtitle>

Common subtitle resource formats include:

- srt;
- vtt;
- other caption formats supported by the implementation.

Subtitles are one of the asset types recognized by OVML.

See: reference/assets.md

## 4. Subtitles and Media

A video or another media element may attach subtitle tracks through a `<subtitles>` child element.

<video src="intro">

`` `<subtitles src="subs_en" language="en-US" />
`` `
</video>

Attributes:

Attribute	Type	Description
src	string	Reference to a subtitle resource or subtitle text
language	language tag	Language of the attached subtitle track

The `<subtitles>` element attaches one or more caption tracks to the media element.

The Player determines how the tracks are synchronized, selected, and presented.

## 5. Subtitle Preferences in <meta>

Project-level subtitle presentation preferences are defined in the `<preferences>` element of `<meta>`.

<meta>
`` `<preferences
    showSubtitles="true"
    subtitleFontSize="16"
    subtitleBg="rgba(0,0,0,0.7)"
    subtitleColor="#ffffff" />
`` `</meta>

The preferences attributes:

showSubtitles — whether subtitles should normally be displayed;
subtitleFontSize — preferred subtitle font size;
subtitleBg — preferred subtitle background color;
subtitleColor — preferred subtitle text color.

These preferences are hints to the Player.

They do not require a particular subtitle rendering implementation.

See: reference/meta.md

## 6. Subtitle vs. Text Blocks

Subtitles are presentation-oriented caption content for delivered media.

Text blocks (`<p>` and `<line>`) are script content that participates in speech, timing, and word-by-word presentation.

The same spoken line may be displayed as project text while separate subtitles accompany the exported media.

A Player MAY derive subtitle content from the project's text blocks, or MAY use explicitly declared subtitle resources.

## 7. Subtitle and Text Presentation

When a spoken line is rendered as an on-screen caption, the Player may apply the project's subtitle preferences:

<line char="narrator">
    The forest was silent.
</line>

may be presented as a caption using the font size, background, and color requested by `<preferences>`.

The precise caption rendering pipeline is implementation-dependent.

## 8. Validation

An OVML validator SHOULD verify:

lang, when present, is a valid language tag;
a referenced subtitle resource is recognized as a subtitle-type resource when project context is available;
<subtitles> appears only in contexts where the media model allows subtitle tracks;
the subtitle element is properly opened and closed.

The validator does not determine whether subtitle timing is synchronized with the media content.

## 9. Design Principle

Subtitle content is descriptive; subtitle presentation is a Player responsibility.

OVML describes what subtitles exist and where they are used.

The Player decides how they are rendered, formatted, and synchronized on the target platform.

## 10. Related Documents

See: reference/meta.md
See: reference/assets.md
See: reference/media.md
See: reference/line.md
See: reference/layers.md