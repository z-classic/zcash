Zclassic 1.1.1
==============
[![Twitter Follow](https://img.shields.io/twitter/follow/ZclassicDev.svg?style=social)](https://twitter.com/ZclassicDev)
[![Tweet](https://img.shields.io/twitter/url/https/github.com/ZclassicDev/ZclassicDev.github.io/README.md.svg?style=social)](https://twitter.com/intent/tweet?text=Check%20out%20Zclassic%20community's%20continuity%20plan%20-%20open%20to%20your%20feedback%20/%20suggestions:%20%F0%9F%A4%97%20%9C%A8%20https://github.com/ZclassicDev/ZclassicDev.github.io/README.md)
[![Discord Chat](https://img.shields.io/discord/308323056592486420.svg)](https://discord.gg/45NNrMJ) [![Github All Releases](https://img.shields.io/github/downloads/atom/atom/total.svg?style=flat)](https://github.com/z-classic/zclassic/releases)<br/>
OverWinter Upgrade with Equihash Parameters Modified and Initial Support for Sapling Upgrade Tests

NOTICE, the default ports have changed!
> MainNet: P2P port =  **8033** and RPC port =  **8023**. <br/>
> TestNet: P2P port = **18033** and RPC port = **18023**.

* Latest Windows and Linux binaries are available here: https://github.com/z-classic/zclassic/releases
* GUI / Swing Wallet UI is here: https://github.com/z-classic/zclassic-full-node-wallet/releases <br/>

What is Zclassic?
-----------------
#### Zclassic is financial freedom.

A cryptocurrency with a focus on privacy, it uses the same initial ceremony
parameters generated for [Zcash](https://github.com/zcash/zcash), as well as
zk-SNARKs for transaction shielding. The major change - there is no 20%
[founders' fee](https://blog.z.cash/funding/) taken for mining each block.
More technical details are available in the [Zcash Protocol Specification](https://github.com/zcash/zips/raw/master/protocol/protocol.pdf).

This software is the Zclassic client. It downloads and stores the entire
history of Zclassic transactions; depending on the speed of your computer
and network connection, the synchronization process could take a day or
more, once the blockchain has reached a significant size.

Two main files of interest in this repo are `zcashd` and `zcash-cli`,
which should be renamed to `zcld` and `zcl-cli` for use in the full-node
wallet. The project needs to be built (per the instructions) in order to
generate them.

#### :lock: Security Warnings

See important security warnings on the
[Security Information page](https://z.cash/support/security/).

Zclassic and Zcash are **highly experimental** and under continuous improvement. Use at your own risk.

#### :ledger: Deprecation Policy

This release is considered deprecated 26 weeks after the release day.
There is an automatic deprecation shutdown feature which will halt the node
sometime after this 26-week time period. The automatic feature is based on
block height and can be explicitly disabled in custom builds.

## Getting Started

We have a guide for joining the main Zclassic network:
https://github.com/z-classic/zclassic/wiki/Getting-Started

### Need help? Want to participate in development?
* :blue_book: See the documentation at the **[Zclassic wiki](https://github.com/z-classic/zclassic/wiki)**.
* :incoming_envelope: View the **[issue tracker](https://github.com/z-classic/zclassic/issues)** and request improvement [here: https://github.com/z-classic/zclassic/issues](https://github.com/z-classic/zclassic/issues).
* :woman_technologist: Code review is welcome! You may also send us a
[pull request](https://github.com/z-classic/zclassic/pulls)
if you want to add something here.
* :mag: Chat with our support community or join the development discussions on [**Discord:** https://discord.gg/45NNrMJ](https://discord.gg/45NNrMJ).

Participation in the Zclassic project is subject to a [Code of Conduct](code_of_conduct.md).

Build and Installation
----------------------

### Linux
Get dependencies
```{r, engine='bash'}
sudo apt-get install \
      build-essential pkg-config libc6-dev m4 g++-multilib \
      autoconf libtool ncurses-dev unzip git python \
      zlib1g-dev wget bsdmainutils automake
```

Install
```{r, engine='bash'}
# Build
./zcutil/build.sh -j$(nproc)
# fetch key
./zcutil/fetch-params.sh
# Run
./src/zcashd
```

### Windows
There are two proven ways to build Zclassic for Windows:

* On Linux using [MinGW-w64](https://mingw-w64.org/doku.php) cross compiler
tool chain. Ubuntu 16.04 Xenial is proven to work and the instructions is for
such release.
* On Windows 10 (64-bit version) using
[Windows Subsystem for Linux (WSL)](https://msdn.microsoft.com/commandline/wsl/about)
and the MinGW-w64 cross-compiler toolchain.

With Windows 10, Microsoft released a feature called WSL. It basically allows
you to run a bash shell directly on Windows in an Ubuntu environment. WSL can
be installed with other Linux variants, but as mentioned before, the distro
proven to work is Ubuntu. Follow
[this link](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)
to install WSL (recommended method).

#### Building for Windows 64-Bit
1. Get the usual dependencies:
```{r, engine='bash'}
sudo apt-get update
sudo apt-get install \
      build-essential pkg-config libc6-dev m4 g++-multilib \
      autoconf libtool ncurses-dev unzip git python \
      zlib1g-dev wget bsdmainutils automake make cmake mingw-w64
```

2. Set the default mingw32 gcc/g++ compiler option to posix, and fix other packing problems with Xenial.

```{r, engine='bash'}
sudo apt install software-properties-common
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu zesty universe"
sudo apt update
sudo apt upgrade
sudo update-alternatives --config x86_64-w64-mingw32-gcc
sudo update-alternatives --config x86_64-w64-mingw32-g++
```

3. Install Rust
```{r, engine='bash'}
curl https://sh.rustup.rs -sSf | sh
source ~/.cargo/env
rustup install stable-x86_64-unknown-linux-gnu
rustup install stable-x86_64-pc-windows-gnu
rustup target add x86_64-pc-windows-gnu

vi  ~/.cargo/config
```
and add:
```
[target.x86_64-pc-windows-gnu]
linker = "/usr/bin/x86_64-w64-mingw32-gcc"
```

Note that in WSL, the Zclassic source code must be somewhere in the default
mount file system, i.e `/usr/src/zclassic`, and not on `/mnt/d/`. What this
means is that you cannot build directly on the Windows system.

4. Build for Windows

```{r, engine='bash'}
PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g') # strip out problematic Windows
%PATH% imported var. ONLY FOR WSL
./zcutil/build-win.sh -j$(nproc)
```

5. Installation

After building in WSL, you can make a copy of the compiled executables to a
directory on your Windows file system. This is done the following way

```{r, engine='bash'}
make install DESTDIR=/mnt/c/zcl/zclassic
```
This will install the executables to `c:\zcl\zclassic`

### Mac
Get dependencies
```{r, engine='bash'}
#install xcode
xcode-select --install

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install cmake autoconf libtool automake coreutils pkgconfig gmp wget

brew install gcc5 --without-multilib
```

Install
```{r, engine='bash'}
# Build
./zcutil/build-mac.sh -j$(sysctl -n hw.ncpu)
# fetch key
./zcutil/fetch-params.sh
# Run
./src/zcashd
```


License
-------
For license information see the file [COPYING](COPYING).