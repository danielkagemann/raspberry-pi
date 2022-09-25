# raspberry pi 

## installation

Best thing to start is using the Raspberry Pi Imager. 
https://www.raspberrypi.com/software/

- select your OS (default raspberry pi image) 
- select your sd card
- go to settings to set your hostname, wlan credentials, activate ssh and so on

If all is okay just click on write and wait...

![imager](./screens/imager.png)
![settings](./screens/settings.png)

Now the card is prepared so just put it into your pi and turn it on.

## connect via ssh

As you can see in the screen above I set the hostname to **mini.local**. Connction via ssh should be like

```
ssh pi@mini.local
```

![ssh](./screens/ssh.png)

### mini.local required packages

install chromium browser (might be already installed)
```
sudo apt-get install chromium-browser
```

install unclutter to get rid of mouse cursor

```
sudo apt-get install unclutter
```

after that we need apache for webserver support
```
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
```

### dashboard installation

clone repository if it exits sometime and then just build the stuff. 
copy the content of the build folder to /var/www/html.

Open browser and type http://mini.local

you should see the dashboard

![](./screens/browser.png)


### prepare autostart

open file in editor

```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```

and add the following lines to it.

```
@unclutter
@xscreensaver -no-splash
@xset s off
@xset -dpms
@xset s noblank
# startet Chromium im vollbild und incognito Modus 
@chromium-browser --incognito --kiosk http://localhost
```