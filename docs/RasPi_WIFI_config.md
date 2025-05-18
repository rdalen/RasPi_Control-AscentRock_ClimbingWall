## Configure WIFI on the RasPi
To get access to the RasPi, it must have the WIFI credentials of your WIFI network.
This may already be pre-configured, if not, they will need to be entered first.
- Connect a HDMI Monitor and a USB-keyboard (or IR keyboard with USB Dongle) to the RasPi
- Power up the RasPi (it takes few minutes to boot up). Log in with `username: pi | password: raspberry`
- Open Raspi-Config `sudo raspi-config`
- Select 1-System Options and press Enter
![Image](https://github.com/user-attachments/assets/a1c9d716-3f1b-4cd5-bf11-062dbb230d4a)
- Select S1-Wireless LAN and then follow the on-screen instructions to enter your network’s SSID and password.<br/> When you’re done, select “Finish” on the main menu to close Raspi-Config.
![Image](https://github.com/user-attachments/assets/a823daf3-3cf0-4b75-8198-a495e948614d)
- Finally, reboot the Raspberry Pi to apply the changed settings.
