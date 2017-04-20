# elementary-panel-logomark

Replace the "Applications" text in the panel with the elementary OS logo.

<img src="example.png" width="250">

## usage

### download logo

Download one of the following logos by pasting a command from the table below into the terminal.

Choose logo size according to the "scaling factor" used by `gsettings`. This factor, which can be checked with `gsettings get org.gnome.desktop.interface scaling-factor | sed 's/.* //'`, is used to "zoom in" the user interface. Most users will not have changed this setting from the default, non-zoomed value of `1`, and should therefore download a 20px logo.

 size                      | <img src="example-logo-standard.png" width="30"> standard logo | <img src="example-logo-button.png" width="30"> button logo
:-------------------------:|:--------------------------------------------------------------:|:------------------------------------------------------------:
 20px (`scaling-factor 1`) | [download](https://git.io/v9eJ2)                               | [download](https://git.io/v9eJX)
 40px (`scaling-factor 2`) | [download](https://git.io/v9eJK)                               | [download](https://git.io/v9eJM)
 60px (`scaling-factor 3`) | [download](https://git.io/v9eJi)                               | [download](https://git.io/v9eJ9)

With the logo in place, open `~/.config/gtk-3.0/gtk.css` and add the following line:

```
.logo{background:url("logo.png") no-repeat center/20px;padding:0 10px}
```

...replacing `logo.png` with the logo image filename.

### remove "Applications" text from wingpanel

The "Applications" text label for the application launcher (`slingshot-launcher`) can be removed by modifying the launcher's source code.

Paste the following block of code in the terminal:

```
bash -ec "

# get slingshot-launcher source and the packages required to build it
cd /tmp; sudo rm -rf slingshot-launcher*
apt source slingshot-launcher; sudo apt build-dep slingshot-launcher

# edit source code (remove 'Applications' text and add css class `.logo`)
cd slingshot-launcher*
sed -i '/App/a indicator_label.get_style_context().add_class(\"logo\");' \
src/Slingshot.vala
sed -i 's/Applications/ /' src/Slingshot.vala

# build and install source code
mkdir build; cd build; cmake .. -DCMAKE_INSTALL_PREFIX=/usr; sudo make install"
```

### restart wingpanel

Finally, restart wingpanel by running:

```
bash -c "killall wingpanel; wingpanel &"
```

## reversal

To reverse the effects of the above code, paste this block in the terminal:

```
bash -c "

# reinstall slingshot-launcher
sudo apt install --reinstall slingshot-launcher

# remove css styling
sed -i '/^\.logo/d' ~/.config/gtk-3.0/gtk.css

# restart wingpanel
killall wingpanel; wingpanel &

"
```
