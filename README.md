<div align="center">

<img src="splash.png" width="300" />

# m96-chan/dotfiles

**~ my cozy terminal setup ~**

[![OS](https://img.shields.io/badge/Arch-1793D1?style=flat-square&logo=archlinux&logoColor=white)](https://archlinux.org/)
[![OS](https://img.shields.io/badge/Fedora-51A2DA?style=flat-square&logo=fedora&logoColor=white)](https://fedoraproject.org/)
[![OS](https://img.shields.io/badge/macOS-000000?style=flat-square&logo=apple&logoColor=white)](https://www.apple.com/macos/)
[![Terminal](https://img.shields.io/badge/Kitty-000000?style=flat-square&logo=gnometerminal&logoColor=white)](https://sw.kovidgoyal.net/kitty/)
[![Shell](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Prompt](https://img.shields.io/badge/Starship-DD0B78?style=flat-square&logo=starship&logoColor=white)](https://starship.rs/)

</div>

---

Bash + Kitty + Starship をベースにした個人用 dotfiles。
ターミナル起動時にシステム情報を表示する MOTD、Tokyo Night カラースキーム、AI/Tech ニュースダッシュボードなどを含む。

## 構成

```
.
├── .bashrc              # メイン設定 (MOTD, ghq+fzf, starship 等)
├── .bashrc.aliases      # エイリアス定義
├── .gitconfig.aliases   # Git エイリアス
├── .motd_art            # MOTD 用アスキーアート
├── .config/
│   ├── kitty/kitty.conf # Kitty 設定 (Tokyo Night テーマ)
│   ├── starship.toml    # Starship プロンプト設定
│   └── wtf/config.yml   # WTF ダッシュボード設定
├── setup.sh             # シンボリックリンク作成スクリプト
└── splash.png           # スプラッシュ画像
```

## 必要なツール

| ツール | 用途 |
|--------|------|
| [Kitty](https://sw.kovidgoyal.net/kitty/) | GPU ベースのターミナルエミュレータ |
| [Starship](https://starship.rs/) | カスタマイズ可能なシェルプロンプト |
| [fzf](https://github.com/junegunn/fzf) | ファジーファインダー |
| [ghq](https://github.com/x-motemen/ghq) | リポジトリ管理 (`Ctrl+]` で fzf 連携ジャンプ) |
| [Homebrew](https://brew.sh/) | パッケージマネージャ (Linux でも使用) |
| [nvm](https://github.com/nvm-sh/nvm) | Node.js バージョン管理 |
| [Rust / Cargo](https://rustup.rs/) | Rust ツールチェイン |
| [btop](https://github.com/aristocratos/btop) | リソースモニタ |
| [WTF](https://wtfutil.com/) | ターミナルダッシュボード (ニュース, リソース監視) |
| [circumflex](https://github.com/bensadeh/circumflex) | ターミナル用 Hacker News クライアント |
| [tickrs](https://github.com/tarkah/tickrs) | ターミナル用株価チャート |
| [fortune](https://wiki.archlinux.org/title/Fortune) | MOTD のランダム名言表示 |
| [HackGen Console NF](https://github.com/yuru7/HackGen) | Kitty で使用するフォント (Nerd Fonts 対応) |
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | AI コーディングアシスタント CLI |

## インストール

### 共通 (全 OS)

```bash
# 1. リポジトリをクローン
ghq get m96-chan/dotfiles
# または
git clone https://github.com/m96-chan/dotfiles.git ~/dotfiles

# 2. シンボリックリンクを作成
cd ~/dotfiles  # or $(ghq root)/github.com/m96-chan/dotfiles
chmod +x setup.sh
./setup.sh

# 3. Rust ツールチェインをインストール
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 4. Cargo 経由のツールをインストール
cargo install tickrs
```

### macOS

```bash
# Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# ツール一括インストール
brew install starship fzf ghq nvm btop wtfutil circumflex fortune
brew install --cask kitty

# フォント
brew install --cask font-hackgen-nerd
```

### Arch Linux

```bash
# pacman
sudo pacman -S kitty fzf fortune-mod rust

# AUR (yay)
yay -S starship-bin ghq-bin btop wtfutil-bin circumflex-bin hackgen-nerd-fonts

# または Homebrew (Linuxbrew) 経由で統一する場合
# 下記「Homebrew (Linux 共通)」を参照
```

### Fedora / Bazzite

Bazzite は immutable な OS のため、`rpm-ostree` またはコンテナ (`brew`, `distrobox`) 経由でインストールする。

```bash
# rpm-ostree で入るもの
rpm-ostree install kitty fzf fortune-mod

# Homebrew (Linuxbrew) を使う方法を推奨 (下記参照)
```

### Homebrew (Linux 共通)

`.bashrc` は Linuxbrew 前提で書かれているため、Arch / Fedora 問わず Homebrew で統一するのが最も簡単。

```bash
# Homebrew をインストール
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# シェルに Homebrew を追加 (setup.sh で .bashrc がリンクされていれば自動)
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"

# ツール一括インストール
brew install starship fzf ghq nvm btop wtfutil circumflex fortune
brew install --cask kitty
```

### フォント (HackGen Console NF)

Kitty の設定で `HackGen Console NF` を使用している。手動でインストールする場合:

```bash
# 最新リリースを確認: https://github.com/yuru7/HackGen/releases
# 例:
wget https://github.com/yuru7/HackGen/releases/download/v2.9.0/HackGen_NF_v2.9.0.zip
unzip HackGen_NF_v2.9.0.zip
mkdir -p ~/.local/share/fonts
cp HackGen_NF_v2.9.0/*.ttf ~/.local/share/fonts/
fc-cache -fv
```

### nvm + Node.js

```bash
# Homebrew 経由で nvm をインストール済みなら
nvm install --lts
nvm use --lts
```

## setup.sh の動作

`setup.sh` は以下のシンボリックリンクを作成する:

| リンク先 | リンク元 |
|----------|----------|
| `~/.bashrc` | `.bashrc` |
| `~/.bashrc.aliases` | `.bashrc.aliases` |
| `~/.gitconfig.aliases` | `.gitconfig.aliases` |
| `~/.motd_art` | `.motd_art` |
| `~/.config/starship.toml` | `.config/starship.toml` |
| `~/.config/kitty/kitty.conf` | `.config/kitty/kitty.conf` |
| `~/.config/wtf/config.yml` | `.config/wtf/config.yml` |

> `.gitconfig` 本体は管理対象外。エイリアスのみ管理しているため、以下を `.gitconfig` に追記すること:
>
> ```gitconfig
> [include]
>     path = ~/.gitconfig.aliases
> ```

## カスタマイズ

### MOTD

`.motd_art` を差し替えればターミナル起動時の画像を変更できる。
[chafa](https://hpjansson.org/chafa/) 等で画像をテキスト化して使用:

```bash
chafa --size=48x24 your_image.png > ~/.motd_art
```

### WTF ダッシュボード

`.config/wtf/config.yml` で RSS フィードやモジュールを編集可能。
デフォルトでは Yahoo! ニュース、Hacker News、AI 関連フィードを表示。

```bash
wtfutil
```

## ショートカット

### Bash

| キー | 動作 |
|------|------|
| `Ctrl+]` | ghq + fzf でリポジトリにジャンプ |

### Kitty

| キー | 動作 |
|------|------|
| `Ctrl+Shift+T` | 新しいタブ |
| `Ctrl+Shift+W` | タブを閉じる |
| `Ctrl+Shift+→/←` | タブ移動 |
| `Ctrl+Shift+Enter` | 新しいウィンドウ |
| `Ctrl+Shift+]/[` | ウィンドウ移動 |
| `Ctrl+Shift+=/-/0` | フォントサイズ変更 |
| `Ctrl+Shift+F5` | 設定リロード |

### Git エイリアス

| エイリアス | コマンド |
|-----------|---------|
| `gs` | `git status` |
| `ga` | `git add .` |
| `gc <msg>` | `git commit -m <msg>` |
| `gp` | `git push` |
| `gl` | `git log --oneline -20` |

## License

MIT
