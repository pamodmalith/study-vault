I'll go first. (All screenshots will be examples from my system)

- [Neovim](https://neovim.io/) - Text editor that _can_ be configured to mirror VS Code, with a lot of plugins/help. [Screenshot](https://i.chse.dev/p/linux_cli_tools/neovim.png)
    - _(My neovim config is entirely stolen from a YT series and doesn't have all the features of VS Code)_
- [Fastfetch](https://github.com/fastfetch-cli/fastfetch) - Neofetch/Spec Sheet [Screenshot](https://i.chse.dev/p/linux_cli_tools/fastfetch.png)
- [eza](https://github.com/eza-community/eza) - ls/tree replacement [Screenshot](https://i.chse.dev/p/linux_cli_tools/eza.png) / [Tree Screenshot](https://i.chse.dev/p/linux_cli_tools/eza-tree.png)
- [bat](https://github.com/sharkdp/bat) - cat replacement [Screenshot](https://i.chse.dev/p/linux_cli_tools/bat.png)
- [zoxide](https://github.com/ajeetdsouza/zoxide) - smarter cd
- [dust](https://github.com/bootandy/dust) - du with bar chart [Screenshot](https://i.chse.dev/p/linux_cli_tools/dust.png)
- [duf](https://github.com/muesli/duf) - disk free with pretty graphs [Screenshot](https://i.chse.dev/p/linux_cli_tools/duf.png)
- [btop](https://github.com/aristocratos/btop) - performance monitor [Screenshot](https://i.chse.dev/p/linux_cli_tools/btop.png)
- [tldr](https://github.com/tldr-pages/tldr) - man pages that just tell you what you need to know [Screenshot](https://i.chse.dev/p/linux_cli_tools/tldr.png)
- [ripgrep](https://github.com/BurntSushi/ripgrep) - [Screenshot](https://i.chse.dev/p/linux_cli_tools/ripgrep.png)
- [nala](https://github.com/volitank/nala) - apt but colorful [Screenshot](https://i.chse.dev/p/linux_cli_tools/nala.png)

Couple other goodies:

- `ip -brief --color=auto` [Screenshot](https://i.chse.dev/p/linux_cli_tools/ip.png)
- [Tabby](https://tabby.sh/) - Terminal I'm using


---


I went through a journey recently exploring a bunch of interesting cli/tui tools as well. I've tried all the ones on your list and use most of them still. I'll add a few TUI and CLI:

- [presenterm](https://github.com/mfontanini/presenterm) - terminal slideshow tool ([Screenshot](https://raw.githubusercontent.com/mfontanini/presenterm/master/docs/src/assets/demo.gif))
- [diskonaut](https://github.com/imsnif/diskonaut) - visual representation of file sizes ([Screenshot](https://chibi.zerebos.com//YOf32dXESxI7.png))
- [erdtree](https://github.com/solidiquis/erdtree) - a better tree tool ([Screenshot](https://chibi.zerebos.com//VhzDvqAntKsu.png))
- [fd](https://github.com/sharkdp/fd) - an alternative to `find` with sane defaults (Screenshot)
- [delta](https://github.com/dandavison/delta) - better diffs for git ([Screenshot](https://chibi.zerebos.com//bdMj8vct5GCE.png))
- [fzf](https://github.com/junegunn/fzf) - command-line fuzzy finder (pairs really well with `ripgrep` and `bat`) ([Screenshot](https://chibi.zerebos.com//lMKlrNpBIybo.png))
- [gdu](https://github.com/dundee/gdu) and [dua](https://github.com/byron/dua-cli) are interactive versions of `dust` shown in the OP
- [bottom](https://github.com/ClementTsang/bottom) - my preferred flavor of `top`/`htop`/`btop` ([Screenshot](https://chibi.zerebos.com//CcS3XLOKBucN.png))
- [gping](https://github.com/orf/gping) - `ping` but graph ([Screenshot](https://chibi.zerebos.com//5mPmw11MSsRe.png))
- [hyperfine](https://github.com/sharkdp/hyperfine) - benchmarking tool ([Screenshot](https://chibi.zerebos.com//3UCgFaukz2OB.png))
- [lazygit](https://github.com/jesseduffield/lazygit) - tui for git repos ([Screenshot](https://chibi.zerebos.com//VbxbMtg99m9h.png))
- [micro](https://github.com/zyedidia/micro) - editor like `nano` but powerful ([Screenshot](https://chibi.zerebos.com//UT1UWalzUWfa.png))
- [flow](https://github.com/neurocyte/flow) - a newer editor like vscode but tui ([Screenshot](https://chibi.zerebos.com//KtzBRGW6wxaq.png))
- [otree](https://github.com/fioncat/otree) - `json` and `yaml` tree viewer ([Screenshot](https://chibi.zerebos.com//xp8gZiV0P9Pq.png))
- [sshs](https://github.com/quantumsheep/sshs) - tui for ssh connections ([Screenshot](https://i.imgur.com/vdve7d5.gif))
- [superfile](https://github.com/yorukot/superfile) - powerfile file browser/manager ([Screenshot](https://chibi.zerebos.com//Ovo3spLfXA4R.png))
- [yazi](https://github.com/sxyazi/yazi) - another powerfile file manager ([Screenshot](https://github.com/sxyazi/yazi/assets/17523360/92ff23fa-0cd5-4f04-b387-894c12265cc7))
- [yadm](https://github.com/TheLocehiliosan/yadm) - dotfile manager
- [termscp](https://github.com/veeso/termscp) - a tui for `scp` ([Screenshot](https://chibi.zerebos.com//SEr1iqkFMHbk.png))
- [silicon](https://github.com/Aloxaf/silicon/) - create beautiful code screenshots ([Screenshot](https://raw.githubusercontent.com/Aloxaf/silicon/master/example/example.png))

Most of these are easily available through [homebrew/linuxbrew](https://brew.sh/)

For my shell:

- [zsh](https://www.zsh.org/)
- Addon: [auto suggestions](https://github.com/zsh-users/zsh-autosuggestions)
- Addon: [fast highlighting](https://github.com/zdharma-continuum/fast-syntax-highlighting)
- Prompt [powerlevel10k](https://github.com/romkatv/powerlevel10k) using instant prompt
- No plugin manager or framework like oh-my-zsh or anything

And my terminal emulator:
- Windows: [Windows Terminal](https://github.com/microsoft/terminal) and sometimes [Tabby](https://github.com/Eugeny/tabby) or [Wezterm](https://github.com/wez/wezterm)
- Mac: [Ghostty](https://mitchellh.com/ghostty) and sometimes [iTerm2](https://github.com/gnachman/iTerm2)
- Linux: [Ghostty](https://mitchellh.com/ghostty) and sometimes the default for the DE

---

[df](https://www.google.com/search?client=firefox-b-d&q=df+linux) alternate [dysk](https://dystroy.org/dysk/)

`erdtree` for alternate to tree

