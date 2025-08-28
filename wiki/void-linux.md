# Void Linux

Void Linux is a systemdless Linux distribution.

## Packages

- search: `xbps-query -Rs <name>`
- install: `xbps-install -Su <name>`
- delete: `xbps-remove <name>`
- delete with dependencies: `xbps-remove -ROo <name>`
- Find packages that provide the file `bin/killall`:
    `xlocate bin/killall`
    - To use `xlocate` you first need to install the `xtools`
        package and then run `xlocate -S`

## Services

- List all available services: `ls /etc/sv`
- List enabled services: `ls /var/service`
- Enable `ntpd` service: `ln -sf /etc/sv/ntpd /var/service`
    - Unlike most other distros,
        this is not done by default when installing
        a package that provides a service.
        You need to do it yourself.
- Service management: `sv {status|stop|start|restart} <service name>`

The `vsv` command can also provide a nicer graphical interface to `sv`.
With no arguments it lists all managed services.
To use `vsv`, the `vsv` package needs to be installed.

## Quirks

- `/` is mounted with private propagation by default.
    This breaks podman, docker, and flatpak.
    This can be fixed by running `mount --make-rshared /`
    at boot.
    - [mitakihara](mitakihara.md) is configured to do this automatically


