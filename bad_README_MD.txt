OpenVML Standard
OVML — Open Voice Markup Language
OVML is an open, declarative XML-based standard for describing audiovisual content, interactive narratives, synchronized speech, media assets, scenes, timing, characters, and presentation.

OVML is designed to describe what an audiovisual work should contain and how its elements are structured, while leaving actual rendering and playback to compatible applications.

The standard is designed for:

multimedia documents;
educational content;
presentations;
audiobooks;
podcasts;
short-form video;
game dialogue;
film and anime voiceover;
interactive narratives;
AI-assisted media production.
The official OpenVML ecosystem includes:

OpenVML Standard — the format specification;
OpenVML Player — an open-source runtime for playing OVML projects;
OpenVML Studio — an authoring environment for creating and assembling OVML projects;
OpenVML Cloud / Publishing — optional online services for publishing and distributing OpenVML projects.
Why OVML?
Traditional media formats describe the final result.

For example:

MP4 describes a rendered video;
WAV/MP3 describes rendered audio;
an image file describes a rendered image.
OVML describes the structure and intentions of the work.

A single OVML document can describe:

who speaks;
what they say;
which voice should be used;
when a line starts;
which media asset is displayed;
where the asset is positioned;
how long it is displayed;
which scene is active;
the atmosphere of a scene;
how the camera should behave;
how subtitles are displayed;
how chapters and blocks are organized.
This makes OVML particularly suitable for projects where content is continuously edited, regenerated, translated, re-voiced, or rendered into different target formats.

Core principle
The central architectural idea of OpenVML is:

OVML describes the experience. The Player renders the experience.

The standard therefore does not define a particular rendering engine, audio library, video library, UI framework, operating system, or TTS implementation.

A compatible implementation is free to choose its own:

audio backend;
video backend;
TTS provider;
graphics engine;
UI framework;
storage system;
operating system integration;
rendering pipeline.
The OVML document remains the portable description of the work.

What can be created with OVML?
OVML is intentionally not limited to one type of media.

Lecture / Lesson
Lecture

Educational content with structured chapters, synchronized narration, slides, media assets, timestamps, and a clear narrative.

Suitable for:

e-learning;
tutorials;
university lectures;
training materials;
educational videos.
Presentation
Presentation

Slide-oriented content with synchronized narration, animations, media assets, and speaker notes.

Suitable for:

business presentations;
product demonstrations;
education;
conference presentations;
narrated slide decks.
Shorts / Reels / TikTok
Shorts / Reels

Short-form vertical content with synchronized narration, subtitles, background media, scenes, and timing.

Suitable for:

YouTube Shorts;
Instagram Reels;
TikTok;
promotional clips;
social-media storytelling.
Audiobook
Audiobook

Long-form narrated books with chapters, multiple characters, voices, dialogue, sound effects, music, and navigation.

OVML can describe:

narrator voice;
character voices;
dialogue;
scene structure;
background audio;
chapter navigation;
synchronized text.
This makes OVML suitable for multi-voice and AI-assisted audiobooks.

Game Voiceover
Game Voiceover

Character dialogue and interactive narration with individual voices and structured dialogue blocks.

Potential applications include:

game dialogue;
NPC voices;
interactive stories;
branching narratives;
character voice libraries.
Film Voiceover
Film Voiceover

Multi-character voiceover and dubbing projects with synchronized dialogue, scenes, timing, audio processing, and media assets.

OVML can be used as a structured intermediate representation before final audio/video rendering.

Anime
Anime

Animated stories with scenes, characters, dialogue, emotions, music, sound effects, and visual direction.

OVML provides a structured layer between the screenplay and the final rendered media.

Course
Course

Structured educational programs containing chapters, lessons, narration, slides, media, and interactive elements.

Podcast
Podcast

Conversational audio projects with multiple speakers, introductions, transitions, music, sound effects, and structured episodes.

OVML, OVMZ and OVMV
OpenVML uses different representations for different stages of the media lifecycle.

OVML
OVML is the source description.

It contains the structured description of the work:

characters
scenes
chapters
dialogue
media
timing
camera
presentation
OVML is editable and intended to remain human- and machine-readable.

OVMZ

OVMZ is a project container.

An OVMZ package can contain:

project ├── content.ovml ├── project metadata ├── media resources ├── presets └── pre-rendered or cached resources

OVMZ is intended to make a project portable.

A project can therefore be distributed as a single container while retaining the original OVML structure.

OVMV

OVMV is a rendered video container/output.

When a project is rendered into a final video, the OVML description is converted into a conventional audiovisual result.

Conceptually:

OVML │ │ render ▼ OVMV

Whereas:

OVML │ │ package ▼ OVMZ

preserves the structured project.

OVML document structure

A basic OVML document has the following structure:

<ovml version="2.2" lang="en">

    <meta>
        ...
    </meta>

    <cast>
        ...
    </cast>

    <assets>
        ...
    </assets>

    <script>
        ...
    </script>

</ovml>
The major sections are:

Section Purpose Root document Project metadata and presentation preferences Characters and voice configuration Media resources

<script> Chapters, scenes, and content blocks Additional structural elements are defined by the specification. Scenes OVML supports explicit scene descriptions using the element. A scene can contain visual and narrative information such as: ``` ... ``` The color attribute can be used as a visual hint for UI presentation and processing presets. The atmosphere attribute provides a semantic description of the scene. This information can be used by authoring tools and AI-assisted workflows to help select: background media; music; sound effects; image assets; visual processing presets. A scene therefore provides a semantic layer between the script and the final media composition. Camera Starting with OVML 2.2, the standard also introduces the element. describes visual direction independently from the physical rendering implementation. The purpose is to allow an OVML document to express cinematic intent such as: camera position; framing; shot type; movement; transition; focus; duration; visual emphasis. The exact vocabulary is defined in the OVML 2.2 specification. The important architectural principle is: OVML describes the camera intent; the renderer decides how to realize it. This allows different renderers to implement the same OVML project using different graphics and video technologies. Characters and voices Characters are declared in the section. Example: `````` A character can contain information about: identity; display name; gender; age; narrative role; voice; language; pitch; speech rate; audio processing; video processing; image processing. The standard does not require one specific TTS engine. A compatible implementation may use: local TTS; cloud TTS; operating-system TTS; AI voice providers; user-provided voice engines. Script The <script> section contains the actual content of the work. Content is organized into structural units such as: chapters; scenes; text/dialogue blocks; media blocks; pauses; synchronized word groups. Example: ``` Hello, world! ``` Timing Timing is an important part of OVML. Content can be synchronized using different start modes. afterPrevious The block starts after the previous textual block has finished. duringCurrent The block starts at a specified time relative to the current timing context. absolute The block starts at an absolute position on the timeline. Example: ```