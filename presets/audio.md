# OVML Audio Processing Presets

**OpenVML — Open Voice Markup Language** is an open, XML-based standard for describing structured audiovisual content.

This document defines the currently established audio processing preset format in OVML 2.2.

## `audio_processing`

An audio processing preset uses `<audio_processing>` as its root element.

`` `<audio_processing id="preset_id" name="Preset Name">
    ...
</audio_processing>
`` `
### Attributes

| Attribute | Description                        |
| --------- | ---------------------------------- |
| `id`      | Machine-readable preset identifier |
| `name`    | Human-readable preset name         |

## Processing Order

Audio operations are normally evaluated in the order in which they appear in the document.

For example:

`` `<audio_processing id="example" name="Example">
    <eq enabled="true">
        ...
    </eq>

    <compressor enabled="true">
        ...
    </compressor>

    <gain enabled="true">
        ...
    </gain>

    <convert>
        ...
    </convert>
</audio_processing>
`` `
represents the conceptual chain:

```Source
  ↓
EQ
  ↓
Compressor
  ↓
Gain
  ↓
Convert
`` `
## `enabled`

Processing operations that can be switched on or off use the `enabled` attribute.

`` `<eq enabled="true">
`` `<eq enabled="false">
`` `
An operation with `enabled="false"` is inactive.

## EQ

The `<eq>` element defines frequency-band equalization.

`` `<eq enabled="true">
    <band hz="60" gain_db="1"/>
    <band hz="170" gain_db="0.5"/>
    <band hz="310" gain_db="0"/>
    <band hz="600" gain_db="1.5"/>
    <band hz="1k" gain_db="2"/>
    <band hz="3k" gain_db="3"/>
    <band hz="6k" gain_db="1"/>
    <band hz="12k" gain_db="0.5"/>
</eq>
`` `
### `<band>`

| Attribute | Description                          |
| --------- | ------------------------------------ |
| `hz`      | Frequency of the band                |
| `gain_db` | Gain applied to the band in decibels |

The `hz` value may use compact frequency notation such as:

```60
170
310
600
1k
3k
6k
12k
`` `
## Compressor

The `<compressor>` element controls dynamic range compression.

`` `<compressor enabled="true">
    <threshold>-18</threshold>
    <ratio>3.5</ratio>
    <attack>5</attack>
    <release>150</release>
    <knee>4</knee>
    <makeup_gain>1.2</makeup_gain>
</compressor>
`` `
### Parameters

| Element         | Description           |
| --------------- | --------------------- |
| `<threshold>`   | Compression threshold |
| `<ratio>`       | Compression ratio     |
| `<attack>`      | Attack time           |
| `<release>`     | Release time          |
| `<knee>`        | Knee characteristic   |
| `<makeup_gain>` | Makeup gain           |

The exact unit interpretation is determined by the operation definition and Player implementation.

## Delay

The `<delay>` element applies a delay effect.

`` `<delay enabled="true">
    <time>0.12</time>
    <feedback>0.1</feedback>
    <wet>0.08</wet>
</delay>
`` `
### Parameters

| Element      | Description                |
| ------------ | -------------------------- |
| `<time>`     | Delay time                 |
| `<feedback>` | Feedback amount            |
| `<wet>`      | Wet/effected signal amount |

## Reverb

The `<reverb>` element applies reverberation.

`` `<reverb enabled="true">
    <room_size>0.25</room_size>
    <damping>0.4</damping>
    <wet>0.12</wet>
    <dry>0.9</dry>
</reverb>
`` `
### Parameters

| Element       | Description                |
| ------------- | -------------------------- |
| `<room_size>` | Simulated room size        |
| `<damping>`   | Damping amount             |
| `<wet>`       | Reverberated signal amount |
| `<dry>`       | Original signal amount     |

## Chorus

The `<chorus>` element applies a chorus/modulation effect.

`` `<chorus enabled="true">
    <rate>5.5</rate>
    <depth>0.6</depth>
    <mix>0.4</mix>
</chorus>
`` `
### Parameters

| Element   | Description      |
| --------- | ---------------- |
| `<rate>`  | Modulation rate  |
| `<depth>` | Modulation depth |
| `<mix>`   | Effect mix       |

## Gain

The `<gain>` element adjusts signal level.

`` `<gain enabled="true">
    <db>1.5</db>
</gain>
`` `
### `<db>`

Gain adjustment in decibels.

## Convert

The `<convert>` element defines the output conversion parameters.

`` `<convert>
    <format>mp3</format>
    <bitrate>192</bitrate>
    <sample_rate>44100</sample_rate>
    <channels>2</channels>
</convert>
`` `
### Parameters

| Element         | Description               |
| --------------- | ------------------------- |
| `<format>`      | Output audio format       |
| `<bitrate>`     | Output bitrate            |
| `<sample_rate>` | Output sample rate        |
| `<channels>`    | Number of output channels |

The `convert` operation is part of the actual OVML preset format and is intentionally named `convert`.

## Complete Example: Baron Guard Officer

`` `<audio_processing id="baron_officer_crisp" name="Baron Guard Officer">
    <eq enabled="true">
        <band hz="60" gain_db="1"/>
        <band hz="170" gain_db="0.5"/>
        <band hz="310" gain_db="0"/>
        <band hz="600" gain_db="1.5"/>
        <band hz="1k" gain_db="2"/>
        <band hz="3k" gain_db="3"/>
        <band hz="6k" gain_db="1"/>
        <band hz="12k" gain_db="0.5"/>
    </eq>

    <compressor enabled="true">
        <threshold>-18</threshold>
        <ratio>3.5</ratio>
        <attack>5</attack>
        <release>150</release>
        <knee>4</knee>
        <makeup_gain>1.2</makeup_gain>
    </compressor>

    <delay enabled="true">
        <time>0.12</time>
        <feedback>0.1</feedback>
        <wet>0.08</wet>
    </delay>

    <gain enabled="true">
        <db>0.8</db>
    </gain>

    <convert>
        <format>mp3</format>
        <bitrate>192</bitrate>
        <sample_rate>44100</sample_rate>
        <channels>2</channels>
    </convert>
</audio_processing>
`` `
## Complete Example: Narrator Classic Warm

`` `<audio_processing id="narrator_classic" name="Narrator Classic Warm">
    <eq enabled="true">
        <band hz="60" gain_db="1"/>
        <band hz="170" gain_db="2"/>
        <band hz="310" gain_db="1"/>
        <band hz="600" gain_db="0"/>
        <band hz="1k" gain_db="-0.5"/>
        <band hz="3k" gain_db="-1.5"/>
        <band hz="6k" gain_db="-0.5"/>
        <band hz="12k" gain_db="1"/>
    </eq>

    <compressor enabled="true">
        <threshold>-26</threshold>
        <ratio>3</ratio>
        <attack>7</attack>
        <release>140</release>
        <knee>6</knee>
        <makeup_gain>2</makeup_gain>
    </compressor>

    <reverb enabled="true">
        <room_size>0.25</room_size>
        <damping>0.4</damping>
        <wet>0.12</wet>
        <dry>0.9</dry>
    </reverb>

    <gain enabled="true">
        <db>1.5</db>
    </gain>

    <convert>
        <format>mp3</format>
        <bitrate>192</bitrate>
        <sample_rate>44100</sample_rate>
        <channels>2</channels>
    </convert>
</audio_processing>
`` `
## Complete Example: Husky Drunkard

`` `<audio_processing id="voice_husky_drunk" name="Husky Drunkard">
    <eq enabled="true">
        <band hz="60" gain_db="-10"/>
        <band hz="170" gain_db="-5"/>
        <band hz="310" gain_db="2"/>
        <band hz="600" gain_db="6"/>
        <band hz="1k" gain_db="8"/>
        <band hz="3k" gain_db="5"/>
        <band hz="6k" gain_db="-4"/>
        <band hz="12k" gain_db="-10"/>
    </eq>

    <compressor enabled="true">
        <threshold>-22</threshold>
        <ratio>5</ratio>
        <attack>1</attack>
        <release>50</release>
        <knee>2</knee>
        <makeup_gain>4</makeup_gain>
    </compressor>

    <chorus enabled="true">
        <rate>5.5</rate>
        <depth>0.6</depth>
        <mix>0.4</mix>
    </chorus>

    <gain enabled="true">
        <db>1</db>
    </gain>

    <convert>
        <format>mp3</format>
        <bitrate>192</bitrate>
        <sample_rate>44100</sample_rate>
        <channels>2</channels>
    </convert>
</audio_processing>
`` `
## Current Audio Vocabulary

The following audio operations have a concrete XML representation in the current OVML 2.2 preset examples:

* `eq`
* `compressor`
* `delay`
* `reverb`
* `chorus`
* `gain`
* `convert`

Other audio-processing operations may be added to the standard when their XML representation and semantics have been formally established.

---

**OpenVML — Open Voice Markup Language**

**OVML Standard 2.2**
