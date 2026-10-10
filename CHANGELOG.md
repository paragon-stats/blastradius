# Changelog

All notable changes to this plugin are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project
adheres to [Semantic Versioning](https://semver.org).

## [0.1.0] - 2026-10-10

### Added

- `blastradius` skill (open Agent Skills format): sizes the process a change
  earns to what the change can reach rather than to its diff. Heavy is the
  default and automatic on contact with CI or releases, anything a golden pins,
  a public surface or data model, required checks or repository configuration,
  security and collection policy, dependencies, user-visible output, or
  concurrency and ordering; light is a claim the agent must enumerate and state.
  Weight modulates discretionary ceremony only — never verification, the
  project's own gates, or any rule the operator set. Composes with `lastmile`
  (whether you finish) and `ponytail` (how much you build).
- Packaging as a single-plugin marketplace, so the skill installs and updates
  through `claude plugin` instead of a copied directory.
- CI inherited from the `techdocs-authoring` template: manifest-driven SemVer
  release via release-please, structural validation of plugin and skill
  frontmatter, and CodeQL scanning of GitHub Actions workflows.
