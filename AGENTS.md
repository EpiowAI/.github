# Agent Instructions

This repository consumes the Sylphx engineering doctrine from [SylphxAI/doctrine](https://github.com/SylphxAI/doctrine).

Before changing files here:

- Read [PROJECT.md](./PROJECT.md) and [`.doctrine/project.json`](./.doctrine/project.json) for this repository's goal, lifecycle, boundary, public surfaces, and adoption gaps.
- Read `SylphxAI/doctrine` `AGENTS.md`, `PRINCIPLES.md`, and `ADR.md`, then load any triggered standards.
- Keep enterprise policy in `SylphxAI/doctrine`; this repo should contain only EpiowAI organization-level GitHub facts and runtime adapter surfaces.

Do not add product-specific behavior here. Product behavior belongs in the owning product repository and must consume this repository only through the public surfaces listed in the manifest.
