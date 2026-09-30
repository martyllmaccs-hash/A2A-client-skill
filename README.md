# A2A Client Skill for Hermes

A reusable Hermes Agent skill for safely discovering and calling peers through Hermes' built-in A2A client tools.

- **Install/use:** copy `SKILL.md` into a Hermes skills directory as a skill folder, then load `a2a-client-skill` when configuring or troubleshooting A2A calls.
- **Scope:** caller-side discovery, authentication hygiene, bounded calls, task/result verification, and directional testing. Server deployment/network changes are explicitly out of scope.
- **Security:** repository contains reusable instructions only. Keep credentials, `.env` files, machine-specific addresses, and private deployment data out of the repo.
- **Provenance:** adapted from the MIT-licensed Hermes `hermes-vps-operations` skill's A2A runbook and local deployment observations; not an official Hermes/A2A publication. Verify commands against the installed Hermes version.

See [`SKILL.md`](SKILL.md) for the complete instructions.
