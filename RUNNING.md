Running as a background service
===============================

While `evdevremapkeys` can be run directly, it probably makes the most
sense to run it in the background. On a modern distro with Systemd,
this can be done fairly easily by running it as a user service. I've
provided an example as `examples/evdevremapkeys.service`.

This example assumes you're running gnome-shell and so it adds itself
as `WantedBy` gnome-shell when wayland is in use.

Installation
------------

```shell
sudo groupadd uinput
sudo usermod -a -G input sjentzsch
sudo usermod -a -G uinput sjentzsch
[sudo chmod g+rw /dev/uinput]
[sudo chgrp uinput /dev/uinput]
echo 'KERNEL=="uinput", MODE="0660", GROUP="uinput", OPTIONS+="static_node=uinput"' > /lib/udev/rules.d/50-uinput.rules
mkdir -p ~/.config/systemd/user
cp evdevremapkeys.service ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable evdevremapkeys
loginctl enable-linger sjentzsch
```
Next up, reboot your system or run `systemctl --user start evdevremapkeys` to test beforehand.
