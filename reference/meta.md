> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

OVML <meta>

OpenVML Standard 2.2

1. Purpose

The <meta> element contains descriptive information and general presentation preferences for an OVML project.

Metadata identifies the project but does not define its content, timeline, media resources, character voices, or rendering pipeline.

The <meta> element is optional.

A minimal OVML document may therefore omit <meta> entirely.

2. Structure

The general structure is:

<meta>
```xml
<title>Project title</title>
<author>Project author</author>

<preferences
    showSubtitles="true"
    subtitleFontSize="14"
    subtitleBg="rgba(0,0,0,0.7)"
    subtitleColor="#ffffff" />
```
</meta>

All child elements of <meta> are optional.

3. <title>

The <title> element contains the human-readable title of the project.

Example:

<title>The Last Summer</title>
Rules
The value is plain text.
It may contain Unicode characters.
It is intended for display in project lists, players, libraries, publishing interfaces, and other user interfaces.
The title does not affect playback or timing.

If <title> is absent, the Player or application may use another available identifier, such as a filename or project ID.

4. <author>

The <author> element identifies the author or creator of the project.

Example:

<author>John Smith</author>

Multiple authors may be represented as text according to the application's conventions.

For example:

<author>John Smith, Maria Brown</author>

The standard does not prescribe a particular author-name format.

The <author> value is informational and does not affect playback.

5. <preferences>

The optional <preferences> element defines general presentation preferences for the project.

Example:

<preferences
```xml
showSubtitles="true"
subtitleFontSize="16"
subtitleBg="rgba(0,0,0,0.7)"
subtitleColor="#ffffff" />
```

These preferences provide hints to the Player.

They do not require a particular rendering implementation.

For example, a Player may implement subtitles using HTML/CSS, a native text renderer, or another rendering technology while respecting the same OVML preferences.

6. <preferences> Attributes
showSubtitles

Controls whether subtitles should normally be displayed.

Property	Value
Type	boolean
Required	No
Default	true

Allowed values:

true
false

Example:

<preferences showSubtitles="false" />

If omitted, the default value is true.

The Player may provide a user-level setting that overrides this project preference.

subtitleFontSize

Specifies the preferred subtitle font size.

Property	Value
Type	integer
Unit	px
Required	No
Default	14

Example:

<preferences subtitleFontSize="18" />

The value represents the preferred size rather than a guarantee of a particular physical rendering size.

A Player may adapt the value to the target platform, display density, accessibility settings, or available rendering technology.

subtitleBg

Specifies the preferred subtitle background color.

Property	Value
Type	string
Required	No
Default	rgba(0,0,0,0.7)

The value uses a CSS-compatible color representation.

Example:

<preferences
```xml
subtitleBg="rgba(0,0,0,0.7)" />
```

Another valid example:

<preferences
```xml
subtitleBg="#000000" />
```

The Player is responsible for interpreting the value.

subtitleColor

Specifies the preferred subtitle text color.

Property	Value
Type	string
Required	No
Default	#ffffff

Example:

<preferences
```xml
subtitleColor="#ffffff" />
```

The value uses a CSS-compatible color representation.

7. Default Values

If <preferences> is absent, the following defaults apply:

Preference	Default
showSubtitles	true
subtitleFontSize	14
subtitleBg	rgba(0,0,0,0.7)
subtitleColor	#ffffff

Therefore:

<meta>
```xml
<title>Example</title>
```
</meta>

is equivalent, with respect to these preferences, to:

<meta>
```xml
<title>Example</title>

<preferences
    showSubtitles="true"
    subtitleFontSize="14"
    subtitleBg="rgba(0,0,0,0.7)"
    subtitleColor="#ffffff" />
```
</meta>
8. Complete Example
<meta>

```xml
<title>The Last Evening</title>

<author>OpenVML Example</author>

<preferences
    showSubtitles="true"
    subtitleFontSize="16"
    subtitleBg="rgba(0,0,0,0.7)"
    subtitleColor="#ffffff" />
```

</meta>
9. Minimal Example

The smallest useful metadata section may contain only a title:

<meta>
```xml
<title>My Project</title>
```
</meta>

It is also valid to omit the entire <meta> section:

<ovml version="2.2" lang="en">

```xml
<script>
    ...
</script>
```

</ovml>
10. Metadata and Player Behavior

The <meta> section describes project-level preferences.

It does not directly control the Player's implementation.

For example:

<preferences showSubtitles="true" />

means that subtitles are intended to be visible by default.

It does not specify:

which subtitle renderer must be used;
how subtitles are synchronized internally;
which font-rendering engine is required;
how subtitles are composited into video;
how accessibility settings are implemented.

These decisions belong to the Player or Renderer.

11. User Preferences

A Player may allow the user to override project preferences.

For example:

OVML project
    │
```xml
└── showSubtitles = true
         │
         ▼
   Player default
         │
         ▼
   User preference
         │
         ▼
   Actual presentation
```

This allows an OVML project to provide sensible defaults without preventing the Player from adapting the experience to the user's environment.

For example, a user may disable subtitles even when:

showSubtitles="true"

is specified by the project.

The OVML document therefore expresses the project's intended default, while the Player remains responsible for the final presentation.

12. Unknown Metadata

Implementations should preserve forward compatibility.

An implementation encountering an unknown metadata element or attribute should not reject an otherwise valid OVML document solely because of that unknown metadata.

For example, a future version may introduce:

<meta>
```xml
<title>Example</title>

<futurePreference value="..." />
```
</meta>

An OVML 2.2 implementation that does not understand futurePreference should ignore it rather than treating it as a playback instruction.

Validation of the document's defined OVML 2.2 elements and attributes remains the responsibility of the OVML validator.

13. Scope

The <meta> element is intentionally limited to project identity and general presentation preferences.

The following information belongs elsewhere:

Information	OVML element
Project title	<meta>
Author	<meta>
Subtitle preferences	<meta>
Characters	<cast>
Voice configuration	<character>
Media resources	<assets>
Timing	<script> / script elements
Scenes	<scene>
Camera direction	<camera>
Dialogue	<line>
Images	<img>
Video	<video>
Audio	<audio>

This separation keeps <meta> independent from the project's actual audiovisual behavior.

14. Design Principle

<meta> describes the project.

It does not describe how the project must be rendered.

Metadata expresses project identity and presentation intent; the Player determines the final presentation on the target platform.

This principle allows the same OVML document to be used by different Players, renderers, platforms, and publishing systems without embedding implementation-specific behavior into the standard.