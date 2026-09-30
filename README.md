# Hermes A2A client skill — 0.1

One focused skill: [`SKILL.md`](SKILL.md). Uses Hermes' built-in A2A tools; no custom transport, runtime dependency, server deployment, or network changes.

## Install

Copy `SKILL.md` into an `a2a-client-skill/` folder under the intended profile's skills directory. Start a new session and load `a2a-client-skill`. Keep the repository's test files outside the installed skill folder.

## Changes in 0.1

Configured-name bearer authentication; no duplicate discovery fetch; explicit completion/pending-task handling; bounded retries; independent remote-write verification. History/list are not task-polling tools. Trusted-endpoint requirements are guidance, not a hardened transport implementation.

## Checks

Run `python3 -m unittest discover -s tests -v` (standard library only). These are offline package/manifest regression checks—not live A2A integration tests or performance benchmarks. Commands and tool semantics were checked against [Hermes docs](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/a2a) and the inspected Hermes implementation; recheck installed schemas when versions differ.

## Attribution

Refined from the MIT-licensed Hermes Agent A2A operational guidance. Martin and Hermes Agent maintain this standalone adaptation; it is not an official Hermes publication. Original license attribution is retained in [`LICENSE`](LICENSE). Never commit credentials or machine-private data.
