> **OpenVML — Open Voice Markup Language**
>
> An open, declarative format for describing voice-driven audiovisual content, including dialogue, narration, scenes, media, timing, and synchronization.

# `<audio_processing>` — Audio Processing

**OVML Standard 2.2**

1. Purpose

The `<audio_processing>` element describes audio processing applied to audio material.

It supports the common production effects: equalizer, compressor, limiter, normalizer, noise reduction, reverb, delay, chorus, phaser, flanger, gain, pan, fade, trim, de-esser, de-clicker, pitch shift, and time stretch.

Processing directives are declarative.

The document describes the intended processing. The Player or processing implementation determines how the processing is executed on the target platform.

2. Structure

The general structure is:

```<audio_processing id="my_processor" name="Heavy Bass">

    <eq enabled="true">
        <band hz="60" gain_db="3.0" />
        <band hz="170" gain_db="2.0" />
        <band hz="310" gain_db="1.0" />
        <band hz="600" gain_db="0.5" />
        <band hz="1000" gain_db="0.0" />
        <band hz="3000" gain_db="-1.0" />
        <band hz="6000" gain_db="-2.0" />
        <band hz="12000" gain_db="-3.0" />
    </eq>

    <compressor enabled="true">
        <threshold>-20</threshold>
        <ratio>4.0</ratio>
        <attack>50</attack>
        <release>200</release>
        <knee>10</knee>
        <makeup_gain>3.0</makeup_gain>
    </compressor>

    <gain enabled="true">
        <db>0.0</db>
    </gain>

</audio_processing>
`` `
Each child element is a processing directive.

The `<audio_processing>` element itself is the container.

3. Container Attributes

Attribute	Type	Description
id	ID	Unique identifier of the processing definition
name	string	Human-readable name of the processing definition
enabled	boolean	Enables or disables the whole processing definition

Example:

`` `<audio_processing id="my_processor" name="Heavy Bass">
    ...
</audio_processing>
`` `
When `enabled` is present and false, the processing definition does not participate in the processing chain.

4. Processing Directives

A processing directive is a child element of `<audio_processing>`.

Each directive describes one processing operation and its parameters.

A directive MAY be declared with:

type;
input;
target;
enabled;
parameters.

type

Identifies the kind of processing operation.

In the XML form, the directive element name is the type.

input

Identifies the audio material to which the directive applies.

Allowed scopes include:

Value	Description
master	The final master mix
scene	The audio bus of a single scene
character	A specific character's voice
track	A specific audio track or media element

When absent, the directive applies to the material of its declaration context.

target

Identifies the specific element, track, or bus the directive addresses.

When absent, the directive applies to the whole material of its scope.

enabled

Activates or deactivates the directive.

A directive with enabled="false" does not participate in processing.

parameters

The effect-specific parameters of the directive.

Parameters are declarative hints.

The exact interpretation depends on the selected audio engine.

5. Directive Types

The canonical processing directives are:

Element	Type	Description
eq	equalize	Multi-band equalizer
compressor	compressor	Dynamic range compression
limiter	limiter	Output limiting
normalizer	normalizer	Level normalization
noise_reduction	noise_reduction	Background noise reduction
reverb	reverb	Reverberation
delay	delay	Echo / delay
chorus	chorus	Chorus modulation
phaser	phaser	Phaser modulation
flanger	flanger	Flanger modulation
gain	gain	Generic level adjustment
pan	pan	Stereo panning
fade	fade	Fade in / fade out
trim	trim	Time trimming
deesser	deesser	Sibilance reduction
declicker	declicker	Click removal
pitch_shift	pitch_shift	Pitch shifting
time_stretch	speed	Time stretching (speed)
convert	convert	Output conversion

The task-requested type tokens equalize, compressor, noise_reduction, reverb, pitch_shift, speed, and eq map to the elements eq, compressor, noise_reduction, reverb, pitch_shift, time_stretch, and eq respectively.

6. eq

The eq directive applies a multi-band equalizer.

Band parameters:

`` `<eq enabled="true">
    <band hz="60" gain_db="5.0" />
    <band hz="10000" gain_db="-3.0" />
</eq>
`` `
Parameter	Description	Range
hz	Band center frequency	20 — 20000 Hz
gain_db	Band gain	-24 — +24 dB

7. compressor

The compressor directive compresses the dynamic range.

`` `<compressor enabled="true">
    <threshold>-20</threshold>
    <ratio>4.0</ratio>
    <attack>50</attack>
    <release>200</release>
    <knee>10</knee>
    <makeup_gain>3.0</makeup_gain>
</compressor>
`` `
Parameter	Description	Range
threshold	Compression threshold	-60 — 0 dB
ratio	Compression ratio	1 — 20
attack	Attack time	0.1 — 200 ms
release	Release time	10 — 1000 ms
knee	Knee width	0 — 20 dB
makeup_gain	Gain applied after compression	-12 — +12 dB

8. limiter

The limiter directive prevents the signal from exceeding a ceiling.

`` `<limiter enabled="true">
    <threshold>-0.3</threshold>
    <release>50</release>
</limiter>
`` `
Parameter	Description	Range
threshold	Output ceiling	-20 — 0 dB
release	Release time	10 — 500 ms

9. normalizer

The normalizer directive adjusts the overall level to a target.

`` `<normalizer enabled="true">
    <level>-3.0</level>
</normalizer>
`` `
Parameter	Description	Range
level	Target level	-12 — 0 dB

10. noise_reduction

The noise_reduction directive reduces background noise.

`` `<noise_reduction enabled="true">
    <strength>0.7</strength>
    <method>spectral_subtraction</method>
</noise_reduction>
`` `
Parameter	Description	Range
strength	Reduction strength	0 — 1
method	Reduction method	spectral_subtraction, wiener_filter, noise_gate

11. reverb

The reverb directive adds reverberation.

`` `<reverb enabled="true">
    <room_size>0.7</room_size>
    <damping>0.5</damping>
    <wet>0.3</wet>
    <dry>0.7</dry>
    <width>1.0</width>
</reverb>
`` `
Parameter	Description	Range
room_size	Room size	0 — 1
damping	High-frequency damping	0 — 1
wet	Wet signal mix	0 — 1
dry	Dry signal mix	0 — 1
width	Stereo width	0 — 1

12. delay

The delay directive adds an echo.

`` `<delay enabled="true">
    <time>0.5</time>
    <feedback>0.3</feedback>
    <wet>0.3</wet>
</delay>
`` `
Parameter	Description	Range
time	Delay time	0.01 — 2 s
feedback	Feedback amount	0 — 0.95
wet	Wet signal mix	0 — 1

13. chorus

The chorus directive applies chorus modulation.

`` `<chorus enabled="false">
    <rate>1.5</rate>
    <depth>0.5</depth>
    <mix>0.5</mix>
</chorus>
`` `
Parameter	Description	Range
rate	Modulation rate	0.1 — 10 Hz
depth	Modulation depth	0 — 1
mix	Dry/wet mix	0 — 1

14. phaser

The phaser directive applies phase modulation.

`` `<phaser enabled="false">
    <rate>1.0</rate>
    <depth>0.5</depth>
    <mix>0.5</mix>
</phaser>
`` `
Parameter	Description	Range
rate	Modulation rate	0.1 — 10 Hz
depth	Modulation depth	0 — 1
mix	Dry/wet mix	0 — 1

15. flanger

The flanger directive applies flanging.

`` `<flanger enabled="false">
    <rate>1.0</rate>
    <depth>0.5</depth>
    <mix>0.5</mix>
</flanger>
`` `
Parameter	Description	Range
rate	Modulation rate	0.1 — 10 Hz
depth	Modulation depth	0 — 1
mix	Dry/wet mix	0 — 1

16. gain

The gain directive applies a fixed level adjustment.

`` `<gain enabled="false">
    <db>0.0</db>
</gain>
`` `
Parameter	Description	Range
db	Gain amount	-24 — +24 dB

17. pan

The pan directive positions the signal in the stereo field.

`` `<pan enabled="false">
    <position>0.0</position>
</pan>
`` `
Parameter	Description	Range
position	Pan position	-1 (left) — +1 (right)

18. fade

The fade directive applies fade in and fade out.

`` `<fade enabled="false">
    <in_duration>0.0</in_duration>
    <out_duration>2.0</out_duration>
    <out_start>300</out_start>
</fade>
`` `
Parameter	Description	Range
in_duration	Fade-in duration	0 — 10 s
out_duration	Fade-out duration	0 — 10 s
out_start	Position where fade-out begins	0 — 600 s

19. trim

The trim directive cuts the material to a time window.

`` `<trim enabled="false">
    <start>0.0</start>
    <end>600.0</end>
</trim>
`` `
Parameter	Description	Range
start	Start time	0 — 600 s
end	End time	0 — 600 s

20. deesser

The deesser directive reduces sibilance.

`` `<deesser enabled="false">
    <threshold>-20</threshold>
    <frequency>7000</frequency>
</deesser>
`` `
Parameter	Description	Range
threshold	De-essing threshold	-40 — 0 dB
frequency	Target frequency	1000 — 16000 Hz

21. declicker

The declicker directive removes clicks and pops.

`` `<declicker enabled="false">
    <sensitivity>0.5</sensitivity>
</declicker>
`` `
Parameter	Description	Range
sensitivity	Removal sensitivity	0 — 1

22. pitch_shift

The pitch_shift directive changes the pitch without changing the duration.

`` `<pitch_shift enabled="true">
    <semitones>2.0</semitones>
</pitch_shift>
`` `
Parameter	Description	Unit
semitones	Semitone shift	semitones

23. time_stretch

The time_stretch directive changes the speed without changing the pitch.

It corresponds to the speed type.

`` `<time_stretch enabled="true">
    <factor>1.0</factor>
</time_stretch>
`` `
Parameter	Description	Range
factor	Speed factor	0.5 — 2.0

24. convert

The convert directive describes the requested output format.

`` `<convert>
    <format>mp3</format>
    <bitrate>192</bitrate>
    <sample_rate>44100</sample_rate>
    <channels>2</channels>
</convert>
`` `
Parameter	Description	Allowed values
format	Container format	mp3, wav, flac, ogg, aac, m4a
bitrate	Bitrate	32 — 320 kbps
sample_rate	Sample rate	8000 — 192000 Hz
channels	Channel count	1 (mono), 2 (stereo)

25. Application

Processing may be declared at several scopes.

The scopes form an inheritance chain:

`` `Project (<settings>)
          ↓
    Character
          ↓
Scene / block / media element
`` `
A more specific declaration overrides or supplements the more general one.

26. Project-Level Declaration

A project MAY declare default processing in its `<settings>` section.

For example:

`` `<settings>

    <audio_processing>
        <reverb enabled="true">
            <room_size>0.2</room_size>
            <wet>0.1</wet>
        </reverb>
    </audio_processing>

</settings>
`` `
Project-level processing applies as the default to the project's audio material unless a more specific declaration overrides it.

27. Character-Level Declaration

A character MAY reference an audio-processing preset.

`` `<character
    id="vestfal"
    name="Vestfal"
    audioProcessorId="preset_1"
    audioProcessorName="Warm Voice"
    audioProcessorFile="presets/audio/WarmVoice.ovml" />
`` `
The referenced preset is applied to the character's voice.

See: reference/character.md

With the `input` model, character-level processing uses input="character".

28. Media and Block-Level Declaration

A media element MAY reference a processing preset by the `processing` attribute:

`` `<audio
    src="voice_chapter_1"
    action="play"
    processing="my_processor" />
`` `
Processing may also be declared inline as child directives of the media element:

`` `<audio src="music_theme" action="play" volume="0.5">

    <eq>
        <band hz="60" gain_db="5.0" />
        <band hz="10000" gain_db="-3.0" />
    </eq>

    <compressor>
        <threshold>-15</threshold>
        <ratio>3.0</ratio>
    </compressor>

</audio>
`` `
Inline directives apply only to that media element.

With the `input` model, element-level processing uses input="track".

See: reference/media.md

29. Scene-Level Declaration

A scene MAY provide context for processing preset selection.

The scene provides context; it does not directly execute a preset unless an explicit processing declaration or character configuration specifies one.

See: reference/scene.md

30. Layer-Specific Instantiation

Different audio layers may receive different processing.

A project may route processing by layer using the input scopes:

    master      Final master processing (bus of all layers)
    scene       Processing for a scene's audio bus
    character   Processing for a specific character's voice
    track       Processing for an individual audio element

This allows, for example, background music and dialogue to use different reverb, compression, or noise-reduction settings within the same project.

Layer-specific instantiation is resolved by the Player.

31. Design Principle

<audio_processing> describes the intended audio processing.

It does not require a particular:

audio library;
DSP implementation;
FFmpeg pipeline;
codec library;
operating system;
hardware platform.

A Player MAY implement the same operation using different underlying technologies while preserving the semantics described by the declaration.

Processing directives are declarative. The runtime interprets them.

32. Related Documents

See: reference/media.md
See: reference/character.md
See: reference/cast.md
See: reference/video-processing.md
See: reference/image-processing.md
See: ../presets/audio.md
See: ../presets/README.md