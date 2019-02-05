Updated 23/12/2018

# Program the EEPROM for the Pi to recognise our MM1-Hat

## Install necessary software
```
sudo apt install bison build-essential flex git-core device-tree-compiler
```

## Clone the RaspberryPi hats repository
```
git clone https://github.com/raspberrypi/hats.git
cd hats/eepromutils
make
```

## Let’s create the .eep file and flash it to the EEPROM
Copy the eeprom_settings.txt file and mm1-hat.dts file from the MM1-Hat repository and create a mm1-hat.dtb and finally the mm1-hat.eep file.

```
cd ~/
git clone https://github.com/roboticsmasters/mm1-hat-eeprom.git
cd hats/eepromutils
cp ~/mm1-hat-eeprom/eeprom_settings.txt eeprom_settings.txt
cp ~/mm1-hat-eeprom/lowlevel/hats/eepromutils/mm1-hat.dts robotcar-hat.dts
sudo dtc -@ -I dts -O dtb -o mm1-hat.dtb mm1-hat.dts
./eepmake eeprom_settings.txt mm1-hat.eep mm1-hat.dtb
truncate -s 4096 mm1-hat.eep
```

Now flash our MM1-Hat configuration and device tree

```
chmod +x eepflash.sh
sudo ./eepflash.sh -w -t=24c32 -f=mm1-hat.eep
```

## Finalisation
Reboot the Pi to make it read the hats EEPROM data, recognise the MM1-Hat and set it up according to the device tree information.
You can check with the following command and/or connecting an LED to GPIO17:

`cat /proc/device-tree/hat/product`

Thats all. Any Pi will recognise the hat now (and start blinking the LED on GPIO17).
