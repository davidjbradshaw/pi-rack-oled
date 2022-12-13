# pi-rack-oled (U6143_ssd1306)

This version extends the work of [darkgrue](https://github.com/darkgrue/U6143_ssd1306) and [Gosherm](https://github.com/Gosherm/U6143_ssd1306), it provides the following features over the original Uctronics version:

* Displays hostname instead of IP address
* Top row is never deleted
* Correctly display CPU temperature units of measurement
* Added `/proc/stat` CPU calculation and enabled in favor of `top` (should be more accurate)
* Added `df` disk free space calculation and enabled in favor of `statfs()` (should be more portable)
* Refactored the way numbers were being pushed to the display that should result in better formatting
  * Fit decimals by width using significant figures
  * Right-justify fields (against labels)
* Fixed `-Wall` compiler warnings
* More robust error handling
* Added `/contrib` directory, support scripts
  * script to tar up files
  * systemd service script


##  I2C
Begin by enabling the I2C interface:

```bash
sudo raspi-config
```

Choose `Interface Options` | `I2C`, then answer `Yes` to whether you would like the ARM I2C interface to be enabled.

Install Git and library dependencies

```bash
sudo apt update
sudo apt install git 
```

##  Clone project 
```bash
git clone https://github.com/davidjbradshaw/pi-rack-oled.git
```

## Compile 
```bash
cd pi-rack-oled/C
make clean && make 
```

## Run 
```
sudo ./display
```

## Add automatic start script
Copy the binary file to `/usr/local/bin/`:

```bash
sudo cp ./display /usr/local/bin/
```

Choose one of the following configuration options (`systemd` **or** `rc.local`):
 
```bash
sudo cp ../contrib/U6143_ssd1306.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable U6143_ssd1306.service
sudo systemctl start U6143_ssd1306.service
```

**OR** add the startup command to the `rc.local` script (not recommended)

```bash
sudo nano /etc/rc.local
```

and add the command to the rc.local file:

```bash
/usr/local/bin/display &
```

Reboot your system:

```bash
sudo reboot now
```

## For older 0.91 inch LCD without MCU 
For the older version LCD without MCU controller, you can use the Python demo.

Install the dependent library files:

```bash
sudo apt update
sudo apt install python3-pil python3-pip
sudo pip3 install adafruit-circuitpython-ssd1306
```

Test demo:

```bash 
cd U6143_ssd1306/python 
sudo python3 ssd1306_stats.py
```
