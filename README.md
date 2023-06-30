# MSVC Compilation on Linux
> **Warning** This is written for Arch Linux, take the same steps and apply the distro-specific commands to your own distro.
>
> If you don't explicitly **need** msvc compilation on Linux, stick to the gnu toolchain as it's far easier to deal with.

## Steps
1. Install `llvm`, `wine` and `ld` via pacman (`pacman -S llvm wine ld`)
2. Install `xwin` via cargo (`cargo install xwin`)
   - > **Note** You need to have `~/.cargo/bin` setup in your `PATH`.
3. Install the MSVC toolchain via rustup (`rustup target add x86_64-pc-windows-msvc`)
4. Install `xwin` to your home folder: `xwin --accept-license splat --output /home/(YOUR_USERNAME)/.xwin`
5. Navigate to your Rust project and create a `.cargo` folder, with a `config.toml` file inside of it
6. Insert this into `config.toml` before saving and exiting:
```toml
[target.x86_64-pc-windows-msvc]
linker = "lld"
rustflags = [
  "-Lnative=/home/(YOUR_USERNAME)/.xwin/crt/lib/x86_64",
  "-Lnative=/home/(YOUR_USERNAME)/.xwin/sdk/lib/um/x86_64",
  "-Lnative=/home/(YOUR_USERNAME)/.xwin/sdk/lib/ucrt/x86_64"
]
```
7. Build your project with `cargo` and `xwin` with MSVC as the target: `cargo xwin build --release --target x86_64-pc-windows-msvc`
8. Done!

## Note
This is a hassle, and I haven't figured out 32-bit support yet.

Huge thanks to:
- [Bevy](https://bevy-cheatbook.github.io/setup/cross/linux-windows.html) for the LLD Instructions
- [XWin](https://github.com/rust-cross/cargo-xwin) for making the use of MSVC a bit easier
