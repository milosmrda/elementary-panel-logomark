# elementary-panel-logomark

Replace the "Applications" text in the panel with the elementary OS logo.

<img src="example.png" width="250">

## usage

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

### install logo

Download one of the following logos, then move it to the elementary theme directory with `sudo mv logo.png /usr/share/themes/elementary/gtk-3.0/`. (Change `logo.png` to match the name of the downloaded file.)

Choose logo size based on the "scaling factor" used by `gsettings`. (This factor, which can be checked with `gsettings get org.gnome.desktop.interface scaling-factor`, is used to "zoom in" the user interface. The default, non-zoomed value is `1`.)

 size                      | <img src="example-logo-standard.png" width="30"> standard logo | <img src="example-logo-button.png" width="30"> button logo
:-------------------------:|:--------------------------------------------------------------:|:------------------------------------------------------------:
 20px (`scaling-factor 1`) | [download](logos/logo-standard-20px.png)                       | [download](logos/logo-button-20px.png)
 40px (`scaling-factor 2`) | [download](logos/logo-standard-40px.png)                       | [download](logos/logo-button-40px.png)
 60px (`scaling-factor 3`) | [download](logos/logo-standard-60px.png)                       | [download](logos/logo-button-60px.png)

```
bash -ec "

wget https://git.io/vSh8p -O /tmp/logo.svg
sudo mv /tmp/logo.svg /usr/share/themes/elementary/gtk-3.0/logo.svg
grep '^\.logo' ~/.config/gtk-3.0/gtk.css || echo '.logo\
{background:url(\"/usr/share/themes/elementary/gtk-3.0/logo.svg\") \
no-repeat center/20px;padding:0 10px}' >> ~/.config/gtk-3.0/gtk.css"
```

# restart wingpanel
killall wingpanel; wingpanel &

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
