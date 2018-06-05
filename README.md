# navcoin-sigs

This repository contains the hashes of the NavCoin Core releases signed by its developers.

# How to generate a signature

If you are a NavCoin Developer, you should:

- Complete the build process using [Gitian](https://github.com/NAVCoin/navcoin-core/blob/master/doc/gitian-building.md).
- Generate a file with the hashes of the binaries and sign it with your key:
```
  export VERSION=v4.2.0
  export KEY=maxmuster
  sha256sum * > $VERSION.SHA256SUM.asc
  gpg --armor --detach-sig --default-key $KEY --output $VERSION.$KEY.SHA256SUM.sig.asc $VERSION.SHA256SUM.asc
```
- Pull request your signature and hashes.

# How to verify a binary

- Update the keys following [this](https://github.com/NAVCoin/navcoin-core/tree/master/contrib/gitian-keys)
- Verify the hashes signature
```
  export VERSION=v4.2.0
  for file in $VERSION*.sig.asc; do
    gpg --verify $file $VERSION.SHA256SUM.asc &> /dev/null || (echo Bad Signature && exit 1)
  done
  echo Signatures OK
  ```
- Hash the binary and verify the hash is on the hash list
```
  export BINARY=navcoin-4.2.0-osx64.tar.gz
  export VERSION=v4.2.0
  grep `sha256sum $BINARY` $VERSION.SHA256SUM.asc && echo Binary ok || echo Bad binary
```
