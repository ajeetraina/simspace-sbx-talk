# Intro to Docker Sandboxes

An interactive, fully in-browser lab and talk built on
[Simspace](https://github.com/dockersamples/simspace). Everything in the terminal
is simulated — no real `sbx`, Docker, or network — so it runs the same for
everyone, with nothing to install.

**The story:** coding agents are useful *because* they act on their own — plan,
run commands, observe, repeat, hundreds of times without asking. That is also the
risk. Over six short sections you give an agent a **trusted boundary it can't
cross**, then make the inside of that boundary your own: the tools, the agent, the
endpoints, and one file the whole team shares.

It follows the four pillars of the Docker Sandboxes talk:

1. **Why not containers?** — watch an agent escape a container through the Docker socket
2. **Contain** — the same escape fails inside an `sbx` microVM; credentials never enter the box
3. **Control** — a blocked endpoint, the policy log, and one allow rule to keep working
4. **Choice** — swap the agent harness and stack mixin kits, boundary unchanged
5. **Capacity** — one `sbxenv.yaml` the whole team starts from, changed in a line

The lab lives under [`lab/intro-to-docker-sandboxes/`](lab/intro-to-docker-sandboxes/):
`labspace.yaml` (config + seeded virtual filesystem), `simulator.yaml` (command
behaviour), and one markdown file per section. The sibling
[`lab/intro-to-docker-sandboxes-slides/`](lab/intro-to-docker-sandboxes-slides/)
is the talk deck, presented as full-bleed slides. Both are loaded at runtime by a
prebuilt image, so there's no build step for content.

## Author locally

You only need Docker.

```bash
docker compose up dev              # live preview at http://localhost:5173
docker compose run --rm validate   # lint the lab (fails on errors)
```

Edit the files under `lab/` and refresh the browser to see changes:

- `labspace.yaml` — title, terminals, seed files, sections, variables
- `simulator.yaml` — what each command does (scenarios)
- `*.md` — one file per section of instructions

Pin the toolchain to a released version for reproducibility:

```bash
export SIMSPACE_AUTHORING_IMAGE=dockersamples/simspace-authoring:1
```

## Deploy

**GitHub Pages (default):** enable Pages (Settings → Pages → Source: "GitHub
Actions"), then push to `main`. The workflow in
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) validates the lab
and publishes it. Pull requests are validated first by
[`.github/workflows/validate.yml`](.github/workflows/validate.yml).

**As a container:** the [`Dockerfile`](Dockerfile) bases on the runtime image and
swaps in your lab.

```bash
docker build -t sbx-lab .
docker run --rm -p 8080:80 sbx-lab    # http://localhost:8080
```

## Authoring with an AI agent

This repo is set up for agent authoring. In Claude Code, an `authoring-lab` skill
(under `.claude/`) knows the workflow, `docker compose` / `validate-lab` are
pre-allowed, and a hook auto-validates the lab after every edit under `lab/`.
[`CLAUDE.md`](CLAUDE.md) loads the guide automatically.

## Learn more

- Docker Sandboxes docs — <https://docs.docker.com/ai/sandboxes/>
- Sandbox environment files — <https://docs.docker.com/ai/sandboxes/sandbox-environments/>
- Sandbox kit spec — <https://github.com/docker/sandbox-kit-spec>

See [`AGENTS.md`](AGENTS.md) for an authoring cheat-sheet, and the
[Simspace specs](https://github.com/dockersamples/simspace/tree/main/spec) for the
full `simulator.yaml` / `labspace.yaml` reference.
