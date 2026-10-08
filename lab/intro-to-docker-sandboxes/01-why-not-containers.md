# Why not containers?

Containers isolate *applications* very well. But **agents need more access than
applications do** - and every capability you add to make the agent useful chips
away at the container's isolation.

| Give the agent | It can now | Isolation left |
|---|---|---|
| Your source code  `-v $PWD:/work` | Modify files on the host | ▰▰▰▰▰▰▱▱ |
| Network access  `--network host` | Reach any endpoint, internal or external | ▰▰▰▰▰▱▱▱ |
| API keys  `-e API_KEY=…` | Read and send the raw credential | ▰▰▰▱▱▱▱▱ |
| **Docker socket**  `-v /var/run/docker.sock` | **Start containers on the host with full access** | ▰▱▱▱▱▱▱▱ |

The last row is the common one: agents build and test, so people mount the Docker
socket. That single flag hands the agent your host's Docker daemon.

---

## 🔴 Live demo: escaping a container

Here's how an agent is usually given power - a container with your project mounted
**and the Docker socket mounted** so it can build. Run it on **Your machine**:

```bash terminal-id=host
docker run -d --name agent --rm -v /var/run/docker.sock:/var/run/docker.sock -v "$PWD":/work acme/coding-agent
```

The agent is now running in a container. Feels contained. It isn't.

With the socket, the agent can tell the host's Docker daemon to start **another**
container - one that bind-mounts the entire host filesystem at `/host` - and read
anything on your machine. This is the agent reaching your SSH key:

```bash terminal-id=host
docker run --rm -v /:/host alpine cat /host/root/.ssh/id_ed25519
```

That private key lives on **your** machine, outside the container. The "isolated"
agent just read it. From here it could read other projects, tokens, and cloud
credentials, or push a tampered image - all through the socket you mounted so it
could build.

> [!WARNING]
> A container is a boundary for code you trust to stay in its lane. An autonomous
> agent doesn't stay in its lane - and the capabilities that make it useful are the
> same ones that let it out.

---

## Checkpoint

- [ ] The agent container started with the Docker socket mounted
- [ ] Using that socket, it read a private key from the **host** filesystem
- [ ] You can see why "just use a container" isn't enough for an autonomous agent

A sandbox gives the agent the access it needs **without** giving away the host.
That's Pillar 1.
