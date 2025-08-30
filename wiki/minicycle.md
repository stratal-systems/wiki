# Minicycle

Minicycle is a webhook-based CI/CD server that was
developed for use during [Gateau development](gateau-cicd.md).
It is deployed (kind of) on [Mitakihara](mitakihara.md),
and the source code and (some) documentation is available
[on github](https://github.com/stratal-systems/minicycle-rs).
There is also a `minicycle-rs` image on the [Docker Registry](registry.md).

## Features, bugs, quirks

This section is a memorydump of all the features and quirks
in minicycle.
I will turn this into proper documentation eventually.

### Commit signing

All commits must be signed with gpg in order to trigger minicycle
pipeline.
Look in [our page on GPG](gpg.md) for a quick tutorial on setting up gpg
with git.
In order to register your pubkey with minicycle,
you need to add it to the `gpg` trustdb inside minicycle's container.
Contact maybetree or look in 
[Gateay CI/CD](gateau-cicd.md) for instructions.



