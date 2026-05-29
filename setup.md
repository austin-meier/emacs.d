# Setup (macOS + Linux)

This config targets **Emacs 29+** on macOS and Linux. Install items by section —
language toolchains are optional and the config gates LSP hooks on the
corresponding server being on `PATH`, so a missing tool is a silent no-op
rather than a startup error.

## Required

- **Emacs 29+** — needed for built-in tree-sitter and the `*-ts-mode` hooks
  this config relies on.
- **Git** — elpaca uses it to clone every package on first launch.
- **A C compiler** (`cc` / `clang` / `gcc`) — `treesit-auto` builds grammars
  on demand the first time you open a language.

## Strongly recommended

Power the leader bindings under `SPC f`:

- **ripgrep** (`rg`) → `SPC f g` (`consult-ripgrep`)
- **fd** (`fd-find`) → `SPC f s` (`consult-fd`)

| OS               | Command                                |
|------------------|----------------------------------------|
| macOS (Homebrew) | `brew install ripgrep fd`              |
| Debian / Ubuntu  | `sudo apt install ripgrep fd-find`     |
| Fedora           | `sudo dnf install ripgrep fd-find`     |
| Arch             | `sudo pacman -S ripgrep fd`            |

> On Debian/Ubuntu `fd` ships as `fdfind`. Symlink it so emacs finds it as `fd`:
> `mkdir -p ~/.local/bin && ln -s $(which fdfind) ~/.local/bin/fd`

## Fonts

The font block in `Fonts/Text` defaults to **AdwaitaMono Nerd Font** and
no-ops gracefully if missing — but the modeline / dashboard / completion icons
expect *some* Nerd Font installed.

```sh
# macOS
brew install --cask font-adwaita-mono-nerd-font

# Linux — grab a release from https://www.nerdfonts.com, drop the .ttf files
# into ~/.local/share/fonts/, then:
fc-cache -fv
```

After installing the typeface, run `M-x nerd-icons-install-fonts` once to
fetch the icon glyph fonts used by `nerd-icons`, `doom-modeline`, and
`dashboard`.

## Language toolchains (install only what you use)

### Rust

```sh
# macOS
brew install rustup-init && rustup-init

# Linux
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

rustup component add rust-analyzer
```

Enables `rust-ts-mode` + LSP and the `SPC e` cargo bindings.

### TypeScript / JavaScript / TSX

Needs Node + npm (via `nvm`, Homebrew, or your package manager), then:

```sh
npm install -g typescript typescript-language-server
```

### JSON

```sh
npm install -g vscode-langservers-extracted
```

Provides `vscode-json-language-server`.

### Clojure

- **Java 17+**
  - macOS: `brew install --cask temurin`
  - Linux: `sudo apt install openjdk-17-jdk` / `sudo dnf install java-17-openjdk` / etc.
- **Clojure CLI** (`clj`)
  - macOS: `brew install clojure/tools/clojure`
  - Linux: see <https://clojure.org/guides/install_clojure>
- **clj-kondo** (surfaced through flycheck)
  - macOS: `brew install borkdude/brew/clj-kondo`
  - Linux: `curl -sLO https://raw.githubusercontent.com/clj-kondo/clj-kondo/master/script/install-clj-kondo && chmod +x install-clj-kondo && sudo ./install-clj-kondo`

### Ada

- `alire` bundles `ada_language_server` and `gnat`.
  - macOS: `brew install alire`
  - Linux: download from <https://alire.ada.dev>

### LaTeX (org-mode auto-preview)

Enables `org-fragtog` — LaTeX fragments render to images as your cursor moves.

```sh
# macOS
brew install --cask mactex-no-gui

# Debian / Ubuntu
sudo apt install texlive

# Fedora
sudo dnf install texlive-scheme-basic
```

## Claude Code IDE

```sh
# macOS / Linux
curl -fsSL https://claude.ai/install.sh | bash
```

`claude-code-ide` only loads when the `claude` binary is on `PATH`. Restart
Emacs after installing.

## First launch

1. Clone this repo to `~/.config/emacs` (the path the config expects).
2. Start Emacs. Elpaca clones itself and every listed package on first run —
   give it a minute or two and watch the `*elpaca-log*` buffer.
3. When `treesit-auto` prompts to install a grammar, accept it for languages
   you use.
4. Once language servers are installed and on `PATH`, reopen the relevant
   buffer (or `M-x revert-buffer`) to trigger `lsp`.
