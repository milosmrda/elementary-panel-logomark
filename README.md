# elementary-panel-logomark

Replace the "Applications" text in the panel with the elementary OS logo.

## usage

Just paste the following block of code in the terminal:

```
bash -ec "

# get slingshot-launcher source and the packages required to build it
cd /tmp; apt source slingshot-launcher; sudo apt build-dep slingshot-launcher

# edit source code (remove 'Applications' text and add css class `.logo`)
cd slingshot-launcher*
sed -i 's/Applications\"));/ \"));indicator_label.get_style_context().add_class(\"logo\");/' src/Slingshot.vala

# build and install source code
mkdir build; cd build; cmake .. -DCMAKE_INSTALL_PREFIX=/usr; sudo make install

# download logo and add to panel with css styling
wget https://git.io/vShJn -O /tmp/logo.png
sudo mv /tmp/logo.png /usr/share/themes/elementary/gtk-3.0/logo.png
echo ".logo{background:url('/usr/share/themes/elementary/gtk-3.0/logo.png') \
no-repeat center/20px;padding:0 10px}" >> ~/.config/gtk-3.0/gtk.css

# restart wingpanel
killall wingpanel && wingpanel &

"
```
