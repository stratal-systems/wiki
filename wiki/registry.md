# Docker Registry

STRATAL SYSTEMS operates a
[Docker registry](https://distribution.github.io/distribution/) at
<https://reg.stratal.systems>
in order to distribute containers to clients and build servers.

The registry can be accessed in read-only mode with
the username and password `public`.
If you believe you should have write access to the registry,
contact myabetree.

## Available Images

This list is maintained manually, not
guaranteed to be up to date with the
actual contes of the registry.

### gateau-cuda11

Image for testing Gateau,
based on `docker.io/nvidia/cuda:11.6.1-devel-ubuntu20.04`.
For more information, consult Gateau README.

### gateau-cuda12

Image for testing Gateau,
based on `docker.io/nvidia/cuda:12.9.1-cudnn-devel-oraclelinux9`.
For more information, consult Gateau README.

### gateau-cuda11-play

Image based on `gateau-cuda11` with some enhancements
for interactive use,
mainly for maybetree's personal convenience.

### minicycle-rs

Image containing [Minicycle](minicycle.md),
built from the Dockerfile found on minicycle's github.

