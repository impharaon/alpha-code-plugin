# Repository and development documentation

- This repository owns only Claude Code plugin packaging and integration
  (`.claude-plugin/`, `commands/`, `skills/`). Process follows the user-level
  delivery-* skills (profile `impharaon/alpha-work/governance/delivery-profile.yml`).
- `README.md` covers installation; `docs/README.md` is the documentation entry
  point.
- `.remember/` is ignored local state with zero authority; do not commit it or
  delete user memory without a request.
