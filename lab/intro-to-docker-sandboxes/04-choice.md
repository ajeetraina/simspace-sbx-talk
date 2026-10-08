# Pillar 3 · Choice

> **A restricted sandbox is safe. It also needs your tools, your agent, and your endpoints.**

Most security tools make you trade away flexibility - the more locked down, the less
useful. Sandboxes keep the **boundary fixed** and make the **inside configurable**.
The security comes from the microVM and the proxy; **kits** change what runs inside
without weakening either one.

| | |
|---|---|
| **Agent kit** · one per sandbox | The harness: which coding agent runs and how it starts |
| **Mixin kits** · stack as many as needed | Tools, credentials, and network rules added on top of any agent |
| **Distribution** | Docker Hub or any registry, a Git repo, or a folder in your project |

---

## 🔴 Live demo: running kits from Docker Hub

### Start with one agent kit

You already ran `claude`. Now **swap the harness** - same project, same boundary -
by running a different agent kit. On **Your machine**:

```bash terminal-id=host
sbx run codex
```

Notice what *didn't* change: the microVM, the proxy, the project mount. You swapped
the agent inside the same box.

### Stack mixins for tools and credentials

An agent on its own can't push to GitHub or sign commits. Add mixin kits - each
layers its own tools, credentials, and network rules on top:

```bash terminal-id=host
sbx kit add docker.io/sbx/github-ssh
```

```bash terminal-id=host
sbx kit add docker.io/sbx/ssh-signing
```

```bash terminal-id=host
sbx kit add docker.io/sbx/project-dev-env
```

Inspect what a kit actually contributes before you trust it:

```bash terminal-id=host
sbx kit inspect docker.io/sbx/github-ssh
```

```text no-run-button
PICK ONE AGENT        ADD ANY TOOLS              =  YOUR SANDBOX
● codex                ☑ github-ssh                 github-ssh
○ claude               ☑ ssh-signing                ssh-signing
○ gemini               ☑ project-dev-env            project-dev-env
○ copilot              ☐ code-server                codex
                                                    MicroVM + proxy  (ALWAYS)
```

Change either side - the agent or the tools - and the boundary stays the same.

---

## You can build your own kits

Kits follow an **open specification**. Package your own agent or tooling the same
way the Hub kits are built, and distribute it on any registry, a Git repo, or a
folder in your project.

> **Sandbox kit spec** → <https://github.com/docker/sandbox-kit-spec>
> Includes example kits. Building kits is a session of its own.

---

## Checkpoint

- [ ] Swapped the agent harness with `sbx run codex` - boundary unchanged
- [ ] Stacked mixin kits for GitHub SSH, commit signing, and a project dev env
- [ ] `sbx kit inspect` showed what a kit contributes before adding it
- [ ] You know kits are an open spec you can author and publish yourself

You've assembled a sandbox by hand. Next: capture the whole thing in **one file** the
team shares.
