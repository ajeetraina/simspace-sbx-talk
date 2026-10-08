# Introduction - agents that act

> **A trusted boundary for AI agents - with what you need inside it, shared across your team.**

Coding agents are useful for one reason: they **act on their own**. You give one a
goal and it runs the loop for you - no human in the loop, hundreds of times.

```text no-run-button
  Plan  ─▶  Act  ─▶  Observe  ─▶  Repeat
decide     run cmds   read output   hundreds of
next step  edit files  and errors   times, no asking
```

That autonomy is exactly why it's risky. When the loop goes wrong, the same agent
can:

- delete files outside the project,
- read keys and tokens off your machine,
- send data to an unknown endpoint,
- install a malicious package.

Today developers pick one of two bad options:

| Safe, slow | Fast, risky |
|---|---|
| **Approve every action.** The agent stops to ask permission. *You* become the bottleneck. | **YOLO mode.** Permissions skipped. The agent has the same access as you do. |

> *"I'm babysitting my agents."* — common developer feedback

This lab is the third option: give the agent **everything it needs, inside a
boundary it can't cross**. We'll build that up in four pillars.

| | | |
|---|---|---|
| **01 · Contain** | The boundaries around every agent | *a trusted boundary* |
| **02 · Control** | Policy for developers and organizations | *you decide where it sits* |
| **03 · Choice** | Kits for your agents, tools, and endpoints | *you choose what's inside* |
| **04 · Capacity** | Reproducible environments the whole team shares | *shared with the team* |

---

## How this lab works

Everything in the terminals is **simulated** - no real `sbx`, Docker, or network.
It runs the same for everyone, with nothing to install. There are two terminals,
and which one you use matters:

- **Your machine** - where you run the `sbx` CLI.
- **Sandbox** - a shell *inside* the box, where the agent lives.

Each code block has a **Run** button that targets the right terminal for you. Let's
look at the project the agent has been asked to work on - run this on **Your machine**:

```bash terminal-id=host
ls
```

```bash terminal-id=host
cat README.md
```

A small `web-app`. In this lab you never change the app - you change the **box the
agent runs in**. Next, see why a plain container isn't that box.
