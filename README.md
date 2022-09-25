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

now prepare for building dashboard and homekit accessories with node.

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

# maybe run the export statement displayed in the console

nvm install v18.9.0
```


### dashboard installation

```
cd ~
git clone https://github.com/danielkagemann/raspberry-dashboard.git
cd raspberry-dashboard
yarn install
yarn build
yarn deploy
```

Open browser and type http://mini.local

you should see the dashboard

![](./screens/browser.png)

Use portrait mode by adding `?portrait=1` to the url.

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
# start chromium fullscreen in icognito and kiosk mode
@chromium-browser --incognito --kiosk http://localhost?portrait=1
```

## virtual homekit accessories

### hk-athome

Prepare the storage folder

```bash
mkdir /homekit
sudo chmod 777 /homkit
```

```bash
git clone https://github.com/danielkagemann/hk-athome.git
```

Go to the project directory

```bash
cd hk-athome
```

Install dependencies

```bash
yarn install
```

Build and deploy to /homekit. This copies all data to /homekit.

```bash
yarn build
yarn deploy
```

Add to autostart

```bash
sudo nano /etc/rc.local
HAPSTORAGE=/homekit node /homekit/athome.js&
```

Now you should be able to add this Switch to your homekit environment via PIN code.
Please use code 300-92-008.

## comming soon...

- homekit at home 
- alerts in dashboard from virtual homekit


## Troubleshooting

Please ensure that the network is 2,4Ghz.

If you still can connect via ssh you can use the commandline tool

```
sudo raspi-config
``
