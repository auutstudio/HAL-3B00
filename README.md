# HAL-3B00
An approaching-distance sensor for the Raspberry Pi 3

![HAL3B00_demo1](https://user-images.githubusercontent.com/84039166/190937533-196f0f39-fb8a-4758-8dfb-87f82211b5ac.gif)

This little robot uses the convenience of the [SparkFun Qwiic system](https://www.sparkfun.com/products/15945) and the compact-form [Pimoroni LED Shim](https://shop.pimoroni.com/products/led-shim) to quickly assemble all the necessary the hardware and Python code. It communicates visually how close a person is from your placement of the sensor, with precision to 1 millimeter. Optionally, it can store the real-time measurement in [Pantry Cloud](https://getpantry.cloud/) for another device to check it remotely.

## Parts List

- Raspberry Pi 3B+
    - (This project would work fine on the 3A+, too, but a different case would be needed)
- Micro SD card with standard Raspberry Pi OS
- Appropriate micro-USB Power Supply for the Pi
- SparkFun [Qwiic pHAT 2.0 for Raspberry Pi](https://www.sparkfun.com/products/15945) - *approx. $7*
- SparkFun [Distance Sensor Breakout - 4m, VL53L1X](https://www.sparkfun.com/products/14722) - *approx. $24*
- 1 x SparkFun [Qwiic Cable - 50mm](https://www.sparkfun.com/products/14426) - *$1*
    - Or, buy a [kit of various cable lengths](https://www.sparkfun.com/products/15081) if you want flexibility in the placement of the sensor, away from the case 
- 1 x [2x20 pin Female GPIO Booster Header, 5mm](https://shop.pimoroni.com/products/booster-header?variant=47414520906) - *approx. $2*
- 1 x [2x20 pin Female GPIO Header, 11mm](https://shop.pimoroni.com/products/2x20-pin-gpio-header-for-raspberry-pi-2-b-a?variant=1132812269) - *approx. $2*
- Pomoroni [LED Shim](https://shop.pimoroni.com/products/led-shim?variant=3136952467466) - *approx. $9*
- Relatively rigid double-sided foam tape, such as [these mounting squares](https://smile.amazon.com/Scotch-Mounting-Squares-6-Squares-111-LRG/dp/B00347A8EY/).
    - Ideal dimensions for securing the Distance Sensor to the case would be to cut a length of 2" x 1", but a [1" mounting square](https://smile.amazon.com/Sc%C3%B3tch-Scotch-Inch-Square-White/dp/B004E2RKL2) could work as well.

### Optional Parts
- This project is perfectly operable without an enclosing case, but will not look nearly as cool.
- [Smraza Raspberry Pi 3 B+ Case with Fan/Heat Sinks](https://smile.amazon.com/Smraza-Raspberry-Heatsinks-Supply-Compatible/dp/B07GKXZH7X)
    - This particular kit happens to include a micro-USB Power Supply
- [PopSockets phone grip, HAL9000 design](https://smile.amazon.com/gp/product/B07V98PLHR/) - to have the desired terrifying effect on people born before 1975
    
### Incompatible Parts

- Originally I tried to incorporate a [Fan Shim](https://shop.pimoroni.com/products/fan-shim?variant=29210095812691) for active cooling, but unfortunately it conflicts with the same pin used by the LED Shim, and then the LED Shim is not detected.

## Mod the Smraza case

## Install Raspberry Pi OS (32-bit)

- Follow the normal instructions, such as [these](https://www.raspberrypi.com/documentation/computers/getting-started.html)
- After the GUI loads the first time, you will need to enable the I2C bus:
    - **Pi Start Menu** > **Preferences** > **Raspberry Pi Configuration** > go to the **Interfaces** tab > find I2C in the list and choose the radio button for **Enable** > click **OK**
    - You will need to reboot the Pi to ensure the settings have taken effect.


## Install the accessory libraries

### Pimoroni library for LED Shim
- Follow the instructions on [GitHub](https://github.com/pimoroni/led-shim)
- essentially at a terminal, type:
    ```
    curl https://get.pimoroni.com/ledshim | bash
    ```

### Command line utility for I2C devices
- At a terminal, type:
    ```
    sudo apt-get install -y i2c-tools
    ```    

### SparkFun library for Qwiic Distance Sensor VL53L1X
- These instructions are taken from the "Python Package" section of [SparkFun's Hookup Guide](https://learn.sparkfun.com/tutorials/qwiic-distance-sensor-vl53l1x-hookup-guide/python-package-overview):
    - The repository is hosted on PyPi as the ```sparkfun-qwiic-vl53l1x``` package. Use PyPi installation with the ```pip3``` command:
    
    ```
    sudo pip3 install sparkfun-qwiic-vl53l1x
    ```    
    - the "Python Examples" section of [SparkFun's Hookup Guide](https://learn.sparkfun.com/tutorials/qwiic-distance-sensor-vl53l1x-hookup-guide/python-examples) is extremely useful to familiarize yourself with the code that will be running HAL 3B00.

### Pantry Cloud wrapper
- [pantry_wrapper.py](https://github.com/alexmulligan/pantry_wrapper) on GitHub by [alexmulligan](https://github.com/alexmulligan)
- Because it does not install as a Python package, you will need to manually move/copy the file ```pantry_wrapper.py``` in order for it to be globally available to Python:
    ```
    cd <directory where you downloaded it>
    sudo cp pantry_wrapper.py /usr/lib/python3/dist-packages
    ```   
    
## Check that the two I2C devices as operational
Taking advantage of the ```i2c-tools``` utilities, type:

```
i2cdetect -y 1
```
    
The output should show devices at 0x29 and 0x75, like this:

```   
you@raspberrypi:~/$ i2cdetect -y 1
    0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- 29 -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- 75 -- --
```   
