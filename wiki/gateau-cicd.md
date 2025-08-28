# Gateay CI/CD

It was decided by Arend and maybetree
that to implement continuous integration and deployment for gateau,
github actions is not a sufficent solution and that we should make our own
[boge](bodge.md) using webhooks.

The chief driver for this decision was a desire to avoid reliance on
software and hardware that we do not control.

The result of this is [minicycle](minicycle.md).

## Using

We're still figuring things out,
so everything is kind of difficult and confusing right now
and lots of things are hardcoded.

Basically, we have all the relevant things on `/root/minicycle`
on [Mitakihara](mitakihara.md) right how.
`/root/minicycle/minicycle-rs` is just a cloned version of the repo,
and `/root/minicycle/deploy` contains our run script,
minicycle's volumnes,
and configuration.

## The pipeline

CI pipeline runs on all pushes to `main` and `cicdtest`.
This is configured in `minicycle.toml`.
The pipeline itself is a shell script that tests whether gateau
builds and installs on different version of python
on cuda11 and cuda12.
The code for this is in `.minicycle/entrypoint` and `podman/test-all.sh`
in the Gateau repo.

There is no easy way to view the output right now other than
to check the files in
`/root/minicycle/deploy/repos/gateau/podman/output` and even
then its not very readable.
I'm still working on this!

Minicycle also makes available a not-very-detailed report
of that latest pipeline run on
<https://mitakihara.webhook.stratal.systems/report-latest>.
You can look there too.

## Commit signing

Only signed commits will trigger CI pipeline.

## Running:

- Check if minicycle is running: `podman ps | grep minicycle`
- Stop minicycle: `podman stop minicycle`
- Start minicycle: `cd /root/minicycle/deploy && ./run.sh`
- View logs: `podman log minicycle-rs`
- Poke around the container: `podman exec -ti minicycle-rs sh`


