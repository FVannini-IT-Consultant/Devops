
Install Nerd Fonts
https://www.nerdfonts.com/
**Needs:**  `sudo apt install fontconfig`

- Download a [Nerd Font](http://nerdfonts.com/)
- Unzip and copy to `~/.fonts` or  `~/.local/share/fonts`
- Run the command `fc-cache -fv` to manually rebuild the font cache

>[!Note] **WSL**  the fonts need to be installed in Windows too.
>To install within Windows simply select them all and right-click -> install
In Terminal ->  Settings -> Profile (e.g. Debian) -> Apperance -> Font face -> FontName (e.g. AnonymicePro Nerd Font)

Install Starship
https://starship.rs/guide/
```bash
curl -sS https://starship.rs/install.sh | sh
echo 'eval "$(starship init bash)"' | tee -a ~/.bashrc
source ~/.bashrc
```
Configure a minimal setup
```bash
mkdir -p ~/.config && touch ~/.config/starship.toml
export STARSHIP_CONFIG=~/example/non/default/path/starship.toml
```

Install Starship presets
https://starship.rs/presets/
```bash
starship preset pastel-powerline -o ~/.config/starship.toml
```

Test with
```bash
echo -e "\xf0\x9f\x90\x8d"
echo -e "\xee\x82\xa0"
```

The first line should produce a [snake emoji](https://emojipedia.org/snake/), while the second should produce a [powerline branch symbol (e0a0)](https://github.com/ryanoasis/powerline-extra-symbols#glyphs).