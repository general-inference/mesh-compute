# mesh-compute

Sandboxes for your [Mesh LLM](https://github.com/Mesh-LLM/mesh-llm) agents.
Each sandbox is a small VM on some machine in your private mesh, usually the
one serving your model, with no network unless you allow it.

## Get started

Install the plugin on each machine in your mesh, then restart `mesh-llm`:

```bash
mesh-llm plugins install general-inference/mesh-compute
```

The first sandbox downloads a ~250 MB guest image. To check a machine is
ready:

```bash
~/.mesh-llm/plugins/installed/compute/compute check
```

Your agents now have `sandbox_create`, `sandbox_exec`, `sandbox_read_file`,
`sandbox_write_file` and friends over MCP. Or call them over HTTP:

```bash
curl -s localhost:3131/api/plugins/compute/tools/sandbox_create \
  -d '{"egress": {"mode": "allowlist", "allow": ["@registries"]}}'
# {"sandbox_id": "sbx-…", "node": "…", …}

curl -s localhost:3131/api/plugins/compute/tools/sandbox_exec \
  -d '{"sandbox_id": "sbx-…", "command": "uname -a && pip download six"}'
```

## Requirements

- **Linux**: access to `/dev/kvm` (add yourself to the `kvm` group).
- **macOS**: Apple Silicon.
- **Windows**: WSL2 (`wsl --install`) on x86_64.
- A private mesh: the plugin does not run on public meshes.

## More

[docs/guide.md](docs/guide.md) covers the sandbox config, network egress
rules, operator limits, how placement works, development and releases.
