# Building #

In order to build SigmaVPN, you will need to download the sources, and then compile them using the provided build script.

The build script requires the following:
  * `gcc`, `binutils` and friends
  * `bash` (may produce syntax errors with other shells; if this is the case, substitute '`sh`' with '`bash`' in the instructions below)
  * `libsodium`: [see here](https://github.com/jedisct1/libsodium)


---


**Clone the git repository from Google Code**

```
git clone https://code.google.com/p/sigmavpn/
cd sigmavpn
```

**Use `make` to build the source:**

```
make
```

**Install to the system directories:**

```
sudo make install
```