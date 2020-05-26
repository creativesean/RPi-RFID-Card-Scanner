# RPi Card Scanner
An expandable IoT system that scans an RFID card, authenticates with a database, sends a value over a BLE serial connection, and then completes a designated task.

## *To run the Card Scanner program WITHOUT Bluetooth:*

1.	Open the terminal. Type the following line by line
2.	cd ~
3.	cd Desktop/card_scanner
4.	python clean_no_ble.py


## *To run the Card Scanner program WITH Bluetooth:*

1.	Supply power to the Arduino and make sure that the power light is blinking on the HC05 Bluetooth module
2.	Once the Arduino/HC05 has power, switch over to the Pi and connect to the HC05 through serial connection (see pictures below)
3.	Open the terminal. Type the following line by line
4.	cd ~
5.	cd Desktop/card_scanner
6.	python clean.py

## *To run the python script on boot*

*There are many methods of running a script on boot, but the method I chose leverages autostart*

*If you need access to elements from the X Window System (e.g. you are making a graphical dashboard or game), then you will need to wait for the X server to finish initializing before running your code. One way to accomplish this is to use the `autostart` system.*

> Note: Raspbian is based on the LXDE desktop environment. As a result, the location of the autostart script might be different depending on your particular Linux computer and distribution version.

*After your desktop environment starts (LXDE-pi, in this case), it runs whatever commands it finds in the profile’s autostart script, which is located at /home/pi/.config/lxsession/LXDE-pi/autostart for our Raspberry Pi. Note that the directory pi might be different if you created a new user for your Raspberry Pi. If no user autostart script is found, Linux will run the global /etc/xdg/lxsession/LXDE-pi/autostart script instead.*

*In addition to running commands in autostart, Linux will also look for and execute .desktop scripts found in /home/pi/.config/autostart. The easiest way to execute GUI programs on boot is to create one of these .desktop scripts.*

###### Create a .desktop file

*You do not need root-level access to modify your profile’s (user’s) autostart and .desktop files. In fact, it is recommended that you do not use sudo, as you may affect the permissions of the file (e.g. the file would be owned by root) and make them unable to be executed by autostart (which has user-level permissions).*

Open a terminal, and execute the following commands to create an autostart directory (if one does not already exist) and edit a .desktop file:

```
mkdir /home/pi/.config/autostart
nano /home/pi/.config/autostart/card_scanner.desktop
```

Copy in the following text into the card_scanner.desktop file. Feel free to change the Name and Exec variables.

```
[Desktop Entry]
Type=Application
Name=card_scanner
Exec=/usr/bin/python /home/pi/clean.py <- Insert absolute address of the python script. This may be different for you.
```
> Note: the */usr/bin/python* section of the Exec variable specifies the version of python that your script requires. For this project, only python2 is required, so */usr/bin/python* is okay. If you add some additional functionality and find yourself needing to use python3, replace */usr/bin/python* with */usr/bin/python3*

Save and exit with ctrl + x, followed by y when prompted to save, and then enter. Reboot with:

```
sudo reboot
```

## *Understanding the other files in the “card_scanner” folder*

#### login_manager.csv
This is the file that the main script writes to every time a card is scanned. Every scan is time-stamped and is also        logs the users name/unique identifier. If the user is new, just the time-stamp and unique identifier is logged.

#### stored_data.csv
This file is the local database that stores the first/last name and the unique identifier of the user’s ID. In order to add a user to the database, scan their ID and you will see a message on the LCD screen that reads: “New User .. ID:_______”. Remember this ID number and input the value into a new row in the database. Proceed by entering that users first and last name into the columns that follow “ID”.

#### stored_data_backup.csv
This is a backup of the original “stored_data.csv” file. Given that the python script is reading and writing to the csv file, I wanted to have a backup incase the file unexpectedly corrupted.
