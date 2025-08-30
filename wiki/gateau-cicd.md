# Gateau CI/CD

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
on cuda11 (cuda12 is not supported by mitakihara's GPU).
The code for this is in `.minicycle/entrypoint` and `podman/test-all.sh`
in the Gateau repo.

You can view the status of the latest pipeline run
at <https://mitakihara.webhook.stratal.systems/report-latest>.
In theory, you should be able to refresh it as the pipeline is running,
but in practice the entire minicycle server locks up until
the pipeline is finished (will fix soon).

Once the pipeline is completed the report should look like this:

```
{
  "artifacts": "gateau-1756564880",
  "message": "update minicycle pipeline",
  "ref": "refs/heads/cicdtest",
  "start": {
    "time": 1756564880
  },
  "finish": {
    "time": 1756565076,
    "ok": true
  }
}
```

You can check the output (stdout + stderr) of the pipeline at
<https://mitakihara.webhook.stratal.systems/artifacts/ARTIFACT_NAME/output>
where `ARTIFACT_NAME` is the value of the `artifacts` key in the json.
So for the above example the path to the captured output would be
<https://mitakihara.webhook.stratal.systems/artifacts/gateau-1756564880/output>.

## Debugging

You can view the webhook delivery status in the Settings tab of
the gateau repo: <https://github.com/tifuun/gateau/settings/hooks>

This will list recent webhook deliveries and their status.
Not that right now even successful deliveries are marked
as failed because they time out (again, will fix this soon).

It can also be useful to check minicycle's logs (see below).

## Commit signing

Only signed commits will trigger CI pipeline.
See our page on [GPG](gpg.md) for more info.

To register your pubkey with minicycle,
contact maybetree.
Alternatively,
If you have access to mitakihara,
you can register you GPG kley with minicycle by
`exec`ing into its container and using the `gpg` command present in there.
Simply run `gpg --import` in the container,
paste in your pubkey (you can obtain it with
`gpg -a --export <KEY ID>`),
press `Ctrl`+`d` to mark end of input, and finally `Ctrl`+`d` again
to exit out of the container shell.
It should look like this:

```
[0] root @ mitakihara: /root/minicycle/deploy
>>> podman exec -ti minicycle-rs sh
~/minicycle/deploy # gpg --import
gpg: WARNING: unsafe permissions on homedir '/root/.gnupg'
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEaKcjjRYJKwYBBAHaRw8BAQdABmcmGw2FkQlcQWEOSnmNMPaEtEln+5my2Ikj
SRZ1ore0JW1heWJldHJlZSA8bWF5YmV0cmVlQHN0cmF0YWwuc3lzdGVtcz6ImQQT
FgoAQRYhBPuISxkKe+VCdJeuZA+F1oItZZsLBQJopyONAhsDBQkFo5qABQsJCAcC
AiICBhUKCQgLAgQWAgMBAh4HAheAAAoJEA+F1oItZZsLgnEBAKw2uLbTAjj6F5jJ
3WvyfTXx0ss8pMZuM8Ddm4xInm8oAPwO1RZiI7xy1JQXDCAqjDikZfxCdI1P0vEB
vGmpbCBQD7g4BGinI40SCisGAQQBl1UBBQEBB0CIAxh+lhTXfmllpz9Tcvnz690l
e/9JBRtLqZmVNJ85CwMBCAeIfgQYFgoAJhYhBPuISxkKe+VCdJeuZA+F1oItZZsL
BQJopyONAhsMBQkFo5qAAAoJEA+F1oItZZsLGwQA/ArEtaQ9QV04Xq8tD6QhO6kK
c6sVZqAC3TrOQ0aljnsEAP4j00KIBHT1xdeeSRZ4ce6WwvMP7i3VFQyNp2nq5TfY
Dw==
=fTEW
-----END PGP PUBLIC KEY BLOCK-----

gpg: key 0F85D6822D659B0B: public key "maybetree <maybetree@stratal.systems>" im
ported
gpg: Total number processed: 1
gpg:               imported: 1
```



## Administration

- Check if minicycle is running: `podman ps | grep minicycle`
- Stop minicycle: `podman stop minicycle`
- Start minicycle: `cd /root/minicycle/deploy && ./run.sh`
- View logs: `podman log minicycle-rs`
- Poke around the container: `podman exec -ti minicycle-rs sh`


