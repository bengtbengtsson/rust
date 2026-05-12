# rust

Sandbox for minor Rust exercises.

## Toolchain setup (macOS)

Install the Rust toolchain and `rust-analyzer` LSP:

```sh
brew install rustup-init
rustup-init -y
source "$HOME/.cargo/env"
rustup component add rust-analyzer
```

Verify:

```sh
rustc --version
cargo --version
rust-analyzer --version
```

## Neovim setup

Neovim config lives at `~/.config/nvim/`. A minimal Rust-ready setup needs:

- **Plugin manager** — [lazy.nvim](https://github.com/folke/lazy.nvim)
- **LSP** — [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) configured for `rust-analyzer`
- **Syntax** — [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) with the `rust` parser
- **Completion** — [blink.cmp](https://github.com/Saghen/blink.cmp) or [nvim-cmp](https://github.com/hrsh7th/nvim-cmp)
- **Optional**: [rustaceanvim](https://github.com/mrcjkb/rustaceanvim) for richer Rust integration (inlay hints, `:RustRun`, debugging)
