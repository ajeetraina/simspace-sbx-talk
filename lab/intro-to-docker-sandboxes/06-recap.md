# Recap & get started

You started with an agent that could read a private key off your host through a
mounted Docker socket. You finished with the same agent working freely inside a box
it can't escape, reaching only what you allow, built from kits you chose, described
in one file your whole team shares.

| | | |
|---|---|---|
| **01 · A trusted boundary** | A microVM, a network proxy, and credentials the agent never holds. | *Contain · Control* |
| **02 · Your choice inside it** | Agent and mixin kits for your harness, tools, and endpoints. | *Choice* |
| **03 · Shared with the team** | One environment file in the repo. Update once, everyone follows. | *Capacity* |

The container escape from Section 1 is worth re-running in both terminals to feel the
difference one more time:

```bash terminal-id=box
docker run --rm -v /:/host alpine cat /host/root/.ssh/id_ed25519
```

Same command that walked off with a key on the host. Inside the box, nothing to read.
That's the whole idea: **give the agent everything it needs, inside a boundary it
can't cross.**

---

## Get started for real

Everything here was simulated. To do it on your own machine:

```bash no-run-button
brew install docker/tap/sbx
sbx run claude
```

- **Docker Sandboxes docs** → <https://docs.docker.com/ai/sandboxes/>
- **Environment files** → <https://docs.docker.com/ai/sandboxes/sandbox-environments/>
- **Sandbox kit spec** → <https://github.com/docker/sandbox-kit-spec>

---

## Where to take it next

- Scope the wide-open policy down to exactly the endpoints your agents use, and read
  the policy log after a real run.
- Build a **mixin kit** for a tool your team depends on and publish it to a registry.
- Commit an `sbxenv.yaml` to a real repo and have a teammate start it - confirm you
  both get the identical box.

Contain, Control, Choice, Capacity. The boundary is fixed; the inside is yours.
