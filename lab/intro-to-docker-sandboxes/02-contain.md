# Pillar 1 · Contain

> **The boundaries that keep the agent's actions inside the sandbox.**

A sandbox is a **private machine for your agent**. The agent still runs with full
freedom - but *inside the box*. Three boundaries surround every agent:

| # | Boundary | What it does | Like… |
|---|---|---|---|
| **1** | **MicroVM** | Its own kernel, filesystem, and Docker engine. The host is out of reach. | a separate room with its own walls |
| **2** | **Network proxy** | Every outbound request passes through a proxy that applies policy. | a front desk that checks every exit |
| **3** | **Credential injection** | Keys are added by the proxy on the way out. The agent never holds them. | a front desk that holds the keys |

Let's create the box, then try to break out of it.

---

## Create the sandbox

Run this on **Your machine**:

```bash terminal-id=host
sbx create claude --name agent-box
```

A microVM boots with its own kernel and its own Docker engine. Only `./web-app` is
shared in. Confirm it's there:

```bash terminal-id=host
sbx ls
```

---

## 🔴 Live demo: testing the boundaries

### Try the exact same escape - inside the box

Switch to the **Sandbox** terminal and run the *identical* command from the last
section:

```bash terminal-id=box
docker run --rm -v /:/host alpine cat /host/root/.ssh/id_ed25519
```

Nothing to read. This Docker engine belongs to the **sandbox**, and the host
filesystem was never mounted into it. Boundary 1 held. The same attack that walked
off with a key a moment ago now hits a wall.

### Look for the model API key - it isn't here

Still in the **Sandbox**, print the environment:

```bash terminal-id=box
env
```

No `ANTHROPIC_API_KEY`. The agent inside the box has no credential to read, leak,
or send. So how does it call the model?

### Store the key on the host

The key lives on **Your machine**, held by the proxy - never injected into the box.
Store it:

```bash terminal-id=host
sbx secret set anthropic
```

```bash terminal-id=host
sbx secret ls
```

### Call the model API with no key in the box

Back in the **Sandbox**, call the API. The request leaves with **no** credential
attached:

```bash terminal-id=box
curl https://api.anthropic.com/v1/models
```

`200 OK`. Walk the path the request took:

| 01 · Sandbox | 02 · Proxy | 03 · Proxy | 04 · Internet |
|---|---|---|---|
| Agent calls an API. `Authorization: none` | Policy check: destination allowed? | Key injected: `Authorization: ••••` | API responds `200 OK` |

**The raw key never enters the sandbox.** The agent gets the result, not the
credential.

---

## Checkpoint

- [ ] `sbx create` booted a microVM with its own kernel and Docker engine
- [ ] The container escape from the last section failed inside the box
- [ ] `env` showed no API key inside the sandbox
- [ ] The model API call returned `200 OK` anyway - the proxy injected the key on the way out

The boundary is fixed and always on. Next: **where** that boundary sits is yours to
decide.
