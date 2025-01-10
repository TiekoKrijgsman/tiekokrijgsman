# Create secure compressed archives

Example using [Zstandard](https://facebook.github.io/zstd/), [GNU Privacy Guard](https://gnupg.org/) and [GNU Tar](https://www.gnu.org/software/tar/) to create a secure compressed archive with password AES256 encryption algorithm for a directory called `abc` in the user home directory.

```sh
# Compress and encrypt
tar --create --auto-compress --file "${HOME}/abc.tar.zst" --directory="${HOME}/folder" abc
gpg --cipher-algo AES256 --symmetric "abc.tar.zst"

# Decrypt and extract
gpg --output "abc.tar.zst" --decrypt "abc.tar.zst.gpg"
tar --auto-compress --extract --file "abc.tar.zst"
```
