# GPG

GPG (aka GnuPG, GNU Privacy Guard) is a thing for cyptoigraphically signing
things.
It integrates with git and lets developers sign their commits
so other people and automated systems can check that the code is really
theirs.

[Minicycle](minicycle.md) requires all commits to be signed with a key
it trusts
before running CI pipeline for security reasons.

## Setting up GPG for use with git

0. Install the `gnupg` package from your package manager
0. Generate key: `gpg --gen-key`
0. Check the key ID: `gpg --list-keys`
    The output should look like:
    ```
    /home/maybetree/.gnupg/pubring.kbx
    ----------------------------------
    pub   ed25519 2025-08-21 [SC] [expires: 2028-08-20]
          FB884B190A7BE5427497AE640F85D6822D659B0B
    uid           [ unknown] maybetree <maybetree@stratal.systems>
    sub   cv25519 2025-08-21 [E] [expires: 2028-08-20]
    ```
    The `FB884B....` string is the key ID.
0. Tell git to use your key to sign commits (run insite the repo):
    ```
    git config --local user.signingkey FB884B190A7BE5427497AE640F85D6822D659B0B
    ```
0. Tell git to automatically sign all commits (also run in repo):
    ```
    git config --local commit.gpgsign true
    ```

## Misc commands:

- Export GPG pubkey: `gpg -a --export FB884B190A7BE5427497AE640F85D6822D659B0B'
- Export GPG privkey: `gpg -a --export-secret-keys FB884B190A7BE5427497AE640F85D6822D659B0B'
- Verify that commit is signed: `git verify-commit <COMMIT HASH>`

## Troubleshooting

If you get `inappropriate ioctl for device` error
when trying to `git commit` after enabling commit signing,
add the following to your `.bashrc`
(or the relevant config file for your shell)
and re-open your terminal:

```
export GPG_TTY=$(tty)
```

More info: <https://stackoverflow.com/a/72788147>


