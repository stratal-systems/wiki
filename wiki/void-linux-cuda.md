# CUDA on Void Linux

This article is about configuring CUDA on [Void Linux](void-linux.md).

On Void, proprietary software is quarantined in a separate repo
which needs to be enabled manually:

```
xbps-install -Syu void-repo-nonfree
xbps-install -S
```

Nvidia drivers are split up into `nvidia`, `nvidia470`, and `nvidia390`,
depending on
[how old your card is](https://www.nvidia.com/en-us/drivers/unix/legacy-gpu/).
These packages contain "everything you need",
but on a headless system it might be more prudent to only install the
`-dkms` and `-libs` subpackages. For example:

```
xbps-install nvidia470-dkms nvidia470-libs
```

After installing the DKMS driver, a reboot is REQUIRED to use it.

After reboot,
check that `modprobe nvidia`
works without error.
(this will be called `nvidia` regardless of whther you installed
the modern, 470, or 390 version).

`nvidia-smi` is locked behind full `nivida` (or `nvidia470` or `nvidia390`)
package.
If you installed the full package, you can use it.
Otherwise, `nvtop` from the `nvtop` package provides
similar functionality without pulling in a ton of dependencies.

## Podman

To allow using GPU inside Podman containers,
you need to pass `--gpus=all` to the `podman run` command.

This will not work out of the box and you will see the error:
`Error: setting up CDI devices: unresolvable CDI devices nvidia.com/gpu=all`.

To fix this, run the following:

```
nvidia-ctk cdi generate --output=/etc/cdi/nvidia.yaml
```

## Sources

Most of this information is aggregated from Void Linux
and Nvidia documentation.
These documents provide a more in-depth overview:

- <https://docs.voidlinux.org/config/graphical-session/graphics-drivers/nvidia.html>
- <https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/cdi-support.html>
- <https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html>

