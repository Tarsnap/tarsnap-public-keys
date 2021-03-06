Public signing keys for Tarsnap Backup Inc.
-------------------------------------------

:warning: All of these keys should be signed by
[Colin Percival's GPG key `0x38ceca690c6a6a6e`]
(https://pgp.mit.edu/pks/lookup?search=0x38CECA690C6A6A6E).
Do not trust the keys you download from this repository until you
verify them!

Contents:

- `keys-signing`: all active code-signing keys.
- `keys-packaging`: all active binary package-signing keys.
- `keys-expired`: all expired keys (regardless of whether they
  were signing or packaging).


## Verifying keys

To see information about a key, use:

```
$ gpg keys-signing/tarsnap-signing-key-2016.asc 
pub  4096R/3DA2BCE3 2016-02-18 Tarsnap source code signing key (Colin Percival) <cperciva@tarsnap.com>
```

Ignore the part that says "Tarsnap source code signing key" or
"Colin Percival": these are user-supplied strings, and they would
be the first thing that an attacker would fake!  The important
part is `3DA2BCE3` -- this lets you check it in the
[Web of trust](https://en.wikipedia.org/wiki/Web_of_trust):

- [Key `3DA2BCE3`](https://pgp.mit.edu/pks/lookup?search=0x3DA2BCE3)
  was signed by
- Colin Percival's
  [key `38ceca690c6a6a6e`](https://pgp.mit.edu/pks/lookup?search=0x38CECA690C6A6A6E),
  which was signed by
- the FreeBSD Security Officer's
  [key `15d68804ca6cdfb2`](https://pgp.mit.edu/pks/lookup?search=0x15D68804CA6CDFB2).

> If multiple keys show up when checking `3DA2BCE3`, you can see a
> longer fingerprint with
>
> ```
> $ gpg --with-fingerprint signing-keys/tarsnap-signing-key-2016.asc
> pub  4096R/3DA2BCE3 2016-02-18 Tarsnap source code signing key (Colin Percival) <cperciva@tarsnap.com>
>       Key fingerprint = ECAE BA77 D19D 1EE0 CAF1  628F BC5C FA09 3DA2 BCE3
> ```
>
> although you will need to remove spaces in the fingerprint in
> order to search for that string.


## Debian packaging

To create a `.dsc` file and a `.tar.gz` file:

    dpkg-source -b deb/tarsnap-archive-keyring

This can be done on FreeBSD with the `dpkg` package.

Those files can then be processed into a binary `.deb` file with `pbuilder`,
potentially on a separate (more secure) system.  The binary `.deb` will
install three keyrings:

- `tarsnap-code-signing-keyring.gpg`: signatures for the official source
  tarball.
- `tarsnap-archive-keyring.gpg`: signatures for the official `.deb` packages.
- `tarsnap-experimental-archive-keyring.gpg`: signatures for experimental
  packages.

  The experimental keys are *not* installed as a trusted `apt-get` keyring;
  they can be imported with:

      sudo apt-key add /usr/share/keyrings/tarsnap-experimental-keyring.gpg


## Policy notes

- Once a package is "stable", the version number should be the publication
  date, following the pattern of many other `foo-archive-keyring` packages.
  For example, the updated keyring in August 2019 (which added keys for the
  year 2020) was version 2019.08.24.
