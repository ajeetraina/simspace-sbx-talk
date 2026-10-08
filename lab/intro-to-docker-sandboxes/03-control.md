# Pillar 2 · Control

> **Let the agent work, and decide where the boundary sits.**

Contain is always on. **Control** is the part you tune: what the agent is allowed
to reach. `sbx` ships three network policy profiles, and you add rules on top of
any of them.

| Most restrictive | Default | Least restrictive |
|---|---|---|
| **Locked Down** - only what you explicitly allow | **Balanced** - common dev endpoints allowed, everything else denied | **Open** - outbound allowed, still logged |

See the active profile on **Your machine**:

```bash terminal-id=host
sbx policy ls
```

Every connection is logged, so you can always see what the agent *tried* to reach.

---

## 🔴 Live demo: adjusting policy

### The agent hits a blocked endpoint

Your agent needs data from a third-party API that isn't on the balanced allowlist.
In the **Sandbox**, it tries:

```bash terminal-id=box
curl https://api.weirdthirdparty.io
```

Blocked. The request stopped at the proxy - it never left the box. The agent didn't
crash your machine or exfiltrate anything; it just couldn't reach an endpoint you
hadn't allowed.

### Find it in the policy log

On **Your machine**, look at what the agent tried:

```bash terminal-id=host
sbx policy log
```

There it is: a `DENY` for `api.weirdthirdparty.io`. No guesswork - the log tells you
exactly which destination to decide on.

### Allow it and keep working

You've decided this endpoint is fine. Add an allow rule on top of the profile:

```bash terminal-id=host
sbx policy allow api.weirdthirdparty.io
```

Now retry the **exact same request** in the **Sandbox**:

```bash terminal-id=box
curl https://api.weirdthirdparty.io
```

`200 OK`. Same request, same boundary - you just moved where it sits. The agent kept
working; you stayed in control; nothing was skipped.

---

## Organizations add governance on top

What a developer does locally, an organization does centrally. The layers stack:

| Layer | What it adds |
|---|---|
| **Organization · AI Governance** | Central policy, audit logs, approved agents and kits - enforced across every sandbox |
| **Developer · Local policy** | Choose a profile, add allow/deny rules, read the policy log |
| **Always on · Contain** | MicroVM, network proxy, credential injection |

The boundary at the bottom never moves. Policy decides where the line sits; the line
itself is always there.

---

## Checkpoint

- [ ] `sbx policy ls` showed the active profile (`balanced`)
- [ ] A request to a non-allowlisted endpoint was blocked at the proxy
- [ ] `sbx policy log` showed the `DENY` with the exact destination
- [ ] One `sbx policy allow` rule unblocked it - without touching the boundary

Next: you've controlled what the agent can reach. Now choose **what runs inside**.
