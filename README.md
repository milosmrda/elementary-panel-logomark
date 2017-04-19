# elementary-panel-logomark

Replace the "Applications" text in the panel with the elementary OS logo.

## usage

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

# download logo and add to panel with css styling
wget https://git.io/vSh8p -O /tmp/logo.svg
sudo mv /tmp/logo.svg /usr/share/themes/elementary/gtk-3.0/logo.svg
echo '.logo{background:url(\"/usr/share/themes/elementary/gtk-3.0/logo.svg\") \
no-repeat center/20px;padding:0 10px}' >> ~/.config/gtk-3.0/gtk.css

# restart wingpanel
killall wingpanel; wingpanel &

"
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
