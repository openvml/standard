# Changelog

All notable changes to the OpenVML standard are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

The standard is currently published as a **Draft**; all versions below are considered work in progress and may change without prior notice.

## [Unreleased]

- (placeholder — upcoming changes to the OpenVML standard will be listed here)

## [2.3]

- Added `<world>` — a world canon at the whole-document (project) level, with an arbitrary, extensible set of sections (`<locations>`, `<timeline>`, `<factions>`, `<terms>`, ...).
- Scenes reference canon entities via the `ref` attribute (e.g. `<location ref="...">`) and can define a scene-specific variation via a nested `<variation>`.
- Project types are **not** introduced into the standard — the document is self-describing and interpreted by structure.

## [2.2]

- Added the `assetRef` attribute on media blocks `<img>`, `<video>`, `<audio>` to bind a block to an asset passport from the global catalog.
- Updated the asset passport format with the fields: `id`, `title`, `source`, `author`, `license`, `url`, `tags`, `duration`, `loop`.
- Added a vector embedding mechanism for AI-assisted asset selection.

## [2.1]

- Added the `<scene>` tag — a container for grouping blocks that share a common media context.
- Added the `<blocking>` tag — semantic character relationships.
- Added the `color` attribute on `<scene>` for a custom color palette.
- Added the `atmosphere` attribute for free-form atmosphere description.

## [2.0]

- Base format: `meta`, `settings`, `cast`, `assets`, `script`.