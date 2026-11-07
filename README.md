# My Vault

Plain Markdown notes. Works on any distro, any editor.

## Structure
- `00-inbox/` quick capture. New notes land here. Empty it every Sunday.
- `10-university/` notes by course (year-1, year-2, ...)
- `20-knowledge/` things I learn for myself (programming, linux, devops)
- `30-snippets/` short reusable code and commands
- `90-templates/` note templates
- `99-archive/` old stuff I no longer need

## Rules
1. File names: lowercase-with-hyphens.md
2. One idea per note.
3. Every note starts from a template.
4. Link related notes with [[double brackets]] instead of making new folders.
5. Few plugins. Keep it simple.

## Sunday review (15 min)
- [ ] Empty `00-inbox/` (move each note to its folder or delete it)
- [ ] Add links between related notes
- [ ] Fill in anything marked `TODO`
- [ ] `git add -A && git commit -m "weekly notes"` and `git push`

## Git setup (once)
```bash
cd ~/vault
git init
git add -A && git commit -m "initial vault"
git branch -M main
git remote add origin git@github.com:<username>/vault.git   # PRIVATE repo
git push -u origin main
```
