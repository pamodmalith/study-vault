### 1. Back up any existing neovim config
```bash
mv ~/.config/nvim{,.bak}
mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}
```
### 2. Install LazyVim
```bash
# Clone LazyVim starter
git clone https://github.com/LazyVim/starter ~/.config/nvim

# Remove the .git folder so you can make it your own
rm -rf ~/.config/nvim/.git
```
### 3. The First Launch
```bash
nvim
```
**Watch!**
1. You will see a blue screen with a dashboard.
2. At the bottom, you will see a lot of bars loading and text flashing. **This is normal.**
3. LazyVim is automatically downloading the plugins, themes, and language servers.
4. **Wait** until everything stops moving.

