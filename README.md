# elementary-panel-logomark

Replace the "Applications" text in the panel with the elementary OS logo.

<img src="example.png" width="250">

## usage

### step 1 - download logo

Download one of the following logos by pasting the corresponding command into the terminal.

 size                      | <img src="example-logo-standard.png" width="30"> standard logo  | <img src="example-logo-button.png" width="30"> button logo
:-------------------------:|:---------------------------------------------------------------:|:----------------------------------------------------------:
 20px (`scaling-factor 1`) | `wget https://git.io/v9et4 -O ~/.config/gtk-3.0/logo.png`       | `wget https://git.io/v9etq -O ~/.config/gtk-3.0/logo.png`
 40px (`scaling-factor 2`) | `wget https://git.io/v9etB -O ~/.config/gtk-3.0/logo.png`       | `wget https://git.io/v9etc -O ~/.config/gtk-3.0/logo.png`
 60px (`scaling-factor 3`) | `wget https://git.io/v9etE -O ~/.config/gtk-3.0/logo.png`       | `wget https://git.io/v9etl -O ~/.config/gtk-3.0/logo.png`

Most users should download the 20px size; the larger versions are for displays that have been "zoomed in" with `scaling-factor`. (Check this factor with `gsettings get org.gnome.desktop.interface scaling-factor | sed 's/.* //'`; the default, unzoomed value is `1`.)

### step 2 - replace "Applications" text with logo

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
mkdir build; cd build; cmake .. -DCMAKE_INSTALL_PREFIX=/usr; sudo make install

# add logo to wingpanel with custom css styling
grep '^\.logo' ~/.config/gtk-3.0/gtk.css || echo '.logo{background:url(\"logo.\
png\") no-repeat center/20px;padding:0 10px}' >> ~/.config/gtk-3.0/gtk.css

# restart wingpanel
killall wingpanel; wingpanel &"
```

## reversal

To reverse the effects of the above code, paste this block in the terminal:

```
bash -c "

# reinstall the original slingshot-launcher
sudo apt install --reinstall slingshot-launcher

# remove the custom css styling
sed -i '/^\.logo/d' ~/.config/gtk-3.0/gtk.css

# restart wingpanel
killall wingpanel; wingpanel &

"
```
