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

### Keeping the toolchain up to date

A new stable Rust ships every ~6 weeks. Update periodically (or when a
crate requires a newer `rustc`):

```sh
rustup update          # updates rustc, cargo, and installed components
brew upgrade rustup    # updates rustup itself (Homebrew-managed installs
                       # disable `rustup self update`)
```

## Neovim setup

Neovim 0.12+ ships with everything needed for a working Rust setup — no
external plugin manager required:

- `vim.pack` — built-in plugin manager (`vim.pack.add { ... }`)
- `vim.lsp.config` / `vim.lsp.enable` — built-in LSP configuration
- Tree-sitter library — bundled (parsers for c/lua/vim/markdown only; the
  Rust parser is installed via the `nvim-treesitter` plugin below)
- Auto-completion — trigger manually with `<C-x><C-o>` (omni) or enable
  `vim.lsp.completion` for LSP-driven completion

Config lives at `~/.config/nvim/init.lua`. A minimal Rust-ready `init.lua`:

```lua
-- Plugins (nvim-treesitter provides :TSInstall and the Rust parser)
vim.pack.add {
  { src = 'https://github.com/nvim-treesitter/nvim-treesitter' },
}
-- After first start: run :TSInstall rust

-- LSP for rust-analyzer
vim.lsp.config('rust_analyzer', {
  cmd = { 'rust-analyzer' },
  filetypes = { 'rust' },
  root_markers = { 'Cargo.toml', '.git' },
})
vim.lsp.enable('rust_analyzer')

-- LSP-driven autocompletion on attach
vim.api.nvim_create_autocmd('LspAttach', {
  callback = function(ev)
    local client = vim.lsp.get_client_by_id(ev.data.client_id)
    if client and client:supports_method('textDocument/completion') then
      vim.lsp.completion.enable(true, client.id, ev.buf, { autotrigger = true })
    end
  end,
})
```

Optional plugins (install via `vim.pack.add`):

- [rustaceanvim](https://github.com/mrcjkb/rustaceanvim) — richer Rust
  integration (inlay hints, `:RustRun`, debugging)
- [blink.cmp](https://github.com/Saghen/blink.cmp) — nicer completion UI
