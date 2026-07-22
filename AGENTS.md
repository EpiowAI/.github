# EpiowAI/.github — local agent notes only

Static engineering and delivery standards load from the active Skills runtime
([SylphxAI/skills](https://github.com/SylphxAI/skills) is binding instruction
SSOT). Doctrine and Mission Control are retired historical lineage and must not
be loaded as current instruction authority.

Local truth: [PROJECT.md](./PROJECT.md) and the org-level GitHub facts in this
repository. Keep enterprise policy in Skills; this repo should contain only
EpiowAI organization-level GitHub facts and runtime adapter surfaces.

Do not add product-specific behavior here. Product behavior belongs in the
owning product repository and must consume this repository only through the
public surfaces listed in the local project declaration.

## Local rules

- Prefer the narrowest affected check before full workspace runs.
- Report layers honestly: source · CI · merge · deploy · live proof (do not collapse).
- Never commit secrets, tokens, or private keys.
