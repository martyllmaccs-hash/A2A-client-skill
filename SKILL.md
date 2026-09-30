---
name: a2a-client-skill
description: "Use when configuring or calling another agent through Hermes A2A. Discover peers, keep bearer tokens secret, and verify each call direction independently."
version: 1.0.0
author: Martin / Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [hermes, a2a, agent-to-agent, client, multi-agent]
---

# Hermes A2A Client

Use Hermes' built-in A2A client tools to discover and call a remote A2A agent. A2A is an interoperable JSON-RPC protocol; it is different from Hermes `peer` messaging. This skill covers the caller side and the evidence needed to say a call worked. It does not configure the remote server, publish network ports, or grant access to secrets.

## Use A2A or `hermes peer`?

- Use A2A when the remote agent exposes an A2A endpoint, interoperability matters, or you need the protocol's agent discovery and task semantics.
- Use `hermes peer` for the lightest direct bot-to-bot message when both endpoints are Hermes and its peer API is the intended route.
- A successful inbound reply proves only that inbound exchange. Verify outbound calls independently; links are directional.

## Prerequisites

1. Confirm the remote operator has enabled an authenticated A2A listener and provided its reachable base URL and the caller's credential through a secure channel.
2. From the actual calling runtime/container, verify the endpoint is reachable. A browser on another machine or an Agent Card alone does not prove the caller has a working route.
3. Configure the peer URL and bearer token using the Hermes-supported configuration/secret-manager workflow for the installed version. Keep the token in a secret manager or environment variable; reference it from configuration rather than writing its value there.
4. Enable the A2A client toolset for the intended platform using the installed Hermes CLI's supported command (for current builds: `hermes tools enable a2a --platform cli|telegram|a2a`). Confirm the setting in the active profile. A CLI tool being enabled does not prove the gateway platform has restarted or is live.

Do not put credentials in chat, repository files, shell history, URLs, screenshots, test fixtures, or logs. Never paste a bearer token into a command argument. Do not reuse one token for both directions.

## Client workflow

1. **Check endpoint and card.** Retrieve the remote Agent Card at `/.well-known/agent-card.json` from the calling runtime. Confirm the advertised URL is reachable and matches the endpoint supplied by the operator. A public Agent Card is discovery metadata, not authentication or proof of authorization.
2. **Discover the peer.** Use Hermes `a2a_discover` for the configured URL. Inspect the discovered agent identity and supported capabilities; do not infer permissions from the card.
3. **Make one bounded authenticated call.** Use `a2a_call` with a non-sensitive test request and a clear expected response. Keep the task scope narrow. Do not send secrets or personal data as a connectivity probe.
4. **Inspect the result.** Use `a2a_history` or `a2a_list` as appropriate to determine the task status and response. Distinguish task acceptance from task completion; HTTP success or a task ID alone is not evidence that the requested work finished.
5. **Verify from the receiver when possible.** Require secret-free evidence from the remote side (for example, a matching audit event or exact expected test response). If remote work writes files, require target-side read-back and compare expected content/size or hash.
6. **Record direction and result.** State which caller initiated the exchange, which receiver accepted it, and what response/evidence was observed. Test the reverse direction separately if bidirectional communication is required.

## Common failures

- **Connection refused:** the host is reachable but no service is listening on that port, or the port is not published to the caller's network. Ask the server operator to inspect the listener and host/container port mapping; do not rotate credentials first.
- **Timeout:** the calling runtime has no route, the host is offline, or a firewall blocks traffic. Confirm the actual hostname/address against the intended private network and test from the caller's container.
- **401 / unauthorized:** the receiver rejected the presented credential or caller identity. Check that the receiver trusts the exact caller identity and that the accepted token matches the one configured for this outbound peer. Compare short non-reversible fingerprints if operators need to reconcile values; never print or exchange the values in chat.
- **Agent Card succeeds but call fails:** card retrieval can be unauthenticated. It does not prove that bearer authorization, trusted-peer mapping, task execution, or response delivery works.
- **Task ID returned but no final response:** poll or inspect task status. Do not report completion until the task reaches a terminal successful state and its output is checked.
- **Reply appears to come back the other way:** an A2A response travels as part of the initiated exchange; it does not prove the receiver can independently initiate a new call. Run a separate test from the other caller.

## Security and change boundaries

- Use a unique token per direction and store each side's accepted token under the caller's exact identity.
- Prefer machine-scoped secrets and least-privilege access. A shared secret-manager project may inject unrelated credentials into every machine with access.
- Do not expose the listener publicly merely to make discovery work. Use the intended private network and bind/publish only to its interface when supported.
- This client skill cannot make server-side configuration changes, open firewalls, publish Docker ports, restart a remote gateway, or validate the remote host's effective secret source. Hand those actions to the server operator and verify afterward.
- Treat a peer's “fixed” or “token aligned” message as a report, not proof. Retest from the caller after the receiver's changes are live.

## Verification checklist

- [ ] The caller's runtime can reach the supplied endpoint.
- [ ] The Agent Card identifies the intended peer and advertises a usable endpoint.
- [ ] Hermes client tools are enabled in the intended runtime/profile.
- [ ] An authenticated, non-sensitive test call was initiated by this caller.
- [ ] The task reached successful completion and the expected response was checked.
- [ ] The receiver's side has corroborating secret-free evidence when available.
- [ ] Any required reverse direction was tested separately.
- [ ] No credential value appeared in repository content, commands, logs, or chat.

## Source and scope

This reusable client guide is adapted from the A2A portions of the MIT-licensed Hermes `hermes-vps-operations` skill, especially `references/agent-to-agent-links.md` and `scripts/probe_a2a_peer.py`, plus locally recorded deployment observations. It generalizes deployment-specific details and intentionally omits machine addresses, peer tokens, private endpoints, and account configuration. It is not an official Hermes or A2A project publication. Check the installed Hermes version and current documentation before relying on command names or configuration paths.
