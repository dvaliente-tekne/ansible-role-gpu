# GPU Role

Installs GPU drivers based on hostname. NVIDIA drivers for `yugen`, Intel/Mesa drivers for all other hosts.

## What It Does

1. **Detects GPU type** based on hostname
2. **Installs appropriate drivers** (NVIDIA or Intel/Mesa)
3. **Verifies installation** of all packages

## Requirements

- `community.general` collection (for `pacman` module)
- `ansible-role-hostname` must run before this role (provides `hostname` variable)

```bash
ansible-galaxy collection install community.general
```

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `gpu_nvidia_hostname` | `yugen` | Hostname that uses NVIDIA GPU |
| `gpu_nvidia_packages` | See defaults | NVIDIA packages |
| `gpu_intel_packages` | See defaults | Intel/Mesa packages |

### NVIDIA Packages (yugen)

- lib32-opencl-nvidia-tkg, lib32-vulkan-icd-loader, lib32-nvidia-utils-tkg
- nvidia-open-dkms-tkg, nvidia-settings-tkg, opencl-nvidia-tkg
- vulkan-icd-loader, nvidia-utils-tkg

### Intel/Mesa Packages (other hosts)

- mesa, lib32-mesa, vulkan-intel, lib32-vulkan-intel

## Dependencies

- **ansible-role-hostname**: Provides the `hostname` variable for driver selection

## Example Playbook

```yaml
- hosts: workstations
  roles:
    - ansible-role-hostname   # Sets hostname variable
    - ansible-role-gpu        # Installs appropriate GPU drivers
```

## Tags

| Tag | Description |
|-----|-------------|
| `gpu` | All GPU tasks |
| `config` | Configuration/detection |
| `nvidia` | NVIDIA specific tasks |
| `intel` | Intel/Mesa specific tasks |
| `packages` | Package installation |
| `verify` | Verification tasks |

## Host-Specific Behavior

| Hostname | GPU Type | Packages Installed |
|----------|----------|-------------------|
| yugen | NVIDIA | 8 NVIDIA/TKG packages |
| Others | Intel | 4 Mesa/Vulkan packages |

## License

MIT-0

## Author

dvaliente
