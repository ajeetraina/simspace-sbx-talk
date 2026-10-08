# Pillar 4 · Capacity

> **One developer, one sandbox is a start. Teams need the same sandbox everywhere.**

You just assembled a sandbox by hand - create, add kits, set policy. That works on
one machine, but it lives in your shell history, and every teammate's setup drifts:
different kits, different policies. Scaling up starts with **reproducibility**.

| One developer | A team, by hand | A team, from one file |
|---|---|---|
| Configured by hand. Works on one machine. | Every setup drifts. Different kits, different policies. | Every sandbox matches. Run as many as you need. |

---

## An environment file describes the whole sandbox

An `sbxenv.yaml` committed to the repo declares the **whole** sandbox - the agent,
the workspace, the kits, and the resources - so anyone recreates the identical box
from one file. Save it at the repo root:

```yaml save-as=sbxenv.yaml
# sbxenv.yaml - committed to the repo
schemaVersion: "1"
name: web-app
agent: claude
workspace: ./web-app

kits:
  - sbx/playwright-kit:latest

sandboxOptions:
  cpus: 4
  memory: 8g
```

| Field | What it fixes |
|---|---|
| **agent** | Which harness the sandbox runs |
| **workspace** | The project folder the agent can see |
| **kits** | Tools, credentials, and network rules come with each kit |
| **resources** | CPU and memory for the sandbox |

---

## 🔴 Live demo: sharing a sandbox with the team

### Start a sandbox from the file

The three steps you ran by hand collapse into one. On **Your machine**, this reads
`sbxenv.yaml`, creates the box, applies kits and policy, and attaches you:

```bash terminal-id=host
sbx env run
```

A teammate who clones the repo runs the same command and gets the **same** sandbox -
kits and policy already set. Nothing to remember.

### Change one line, everyone follows

Need Playwright swapped for a different tool, or more memory? Edit the one line in
`sbxenv.yaml`, open a pull request, merge it - and the next sandbox everyone starts
matches.

```text no-run-button
Change the file  →  Open a pull request  →  Merge  →  Everyone pulls
update a kit        reviewed like code       repo holds    next sandbox
or swap the agent                            the new def   each person
                                                            starts matches
```

Tear it down just as declaratively when you're done:

```bash terminal-id=host
sbx env rm
```

> [!NOTE]
> **Cloud Sandboxes** run the *same* `sbxenv.yaml` on Docker's infrastructure, so
> you can start more sandboxes than one laptop can hold. *(Early access - not yet
> available for organization use.)*

---

## Checkpoint

- [ ] `sbxenv.yaml` describes the whole sandbox - agent, workspace, kits, resources
- [ ] `sbx env run` recreated the box from that one file
- [ ] You know a teammate gets the identical sandbox by cloning and running the same command
- [ ] `sbx env rm` tore down everything the file created

One file in the repo. Update once, every sandbox follows.
