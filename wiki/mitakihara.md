# Mitakihara

Mitakihara is a GPU server that we use for gateau testing
and CI.
It is located in Arend's office.

## Hardware

- CPU: Intel Core i7-4800MQ (8) @ 3.70 GHz
- GPU: NVIDIA Quadro K2100M
    - This supports CUDA 11 but not CUDA 12
- MEM: 7GB

## Software

- Mitakihara is running the glibc variant of Void Linux
    - See [Void Linux](void-linux.md) for more info
- proprietary nvidia drivers are installed,
    enabling use of CUDA. Podman is configured to
    allow using the gpu inside containers
    using the `--gpus` flag.
    - See [CUDA on Void Linux](void-linux-cuda.md) for
        details on how this was set up
- There are currently no unprivileged users configured, only `root`
