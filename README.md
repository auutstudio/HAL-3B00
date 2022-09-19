# HAL 3B00
*(pronounced HAL 3-B-Thousand.)* A change-in-distance sensor for the Raspberry Pi 3

![HAL3B00_demo1](https://user-images.githubusercontent.com/84039166/190937533-196f0f39-fb8a-4758-8dfb-87f82211b5ac.gif)

This little robot uses the convenience of the [SparkFun Qwiic system](https://www.sparkfun.com/products/15945) and the compact-form [Pimoroni LED Shim](https://shop.pimoroni.com/products/led-shim) to quickly assemble all the necessary the hardware and Python code. It communicates visually how close an object is from your placement of the sensor. Optionally, it can store the real-time measurement in [Pantry Cloud](https://getpantry.cloud/) for another device to check it remotely.

In its default mode, the Distance Sensor VL53L1X has a maximum range of 4 meters, with the (pretty amazing) ability to measure in 1mm increments. It works fine in the dark as well as indoors; it can have trouble with accurate readings when used outdoors in strong sunlight. It also has a short-distance mode, which limits its range to 1.3 meters but with better resilience to ambient light.

## Use Cases
The purpose of the LED graph here is to demonstrate the sensor's ability and sensitivity. But in a final product, there is no need to display this publicly; HAL 3B00 could simply sense invisibly in real-time; you can work out a notification of some sort when the value meets a specific condition. For example:

- Placed on a nightstand, beaming down (parallel) to the bed, HAL 3B00 would detect if a someone gets up and, unavoidably, crosses the beam. The measurement would go from 4000mm/∞ to something more like 1000-2500mm. People who are caring for a spouse suffering from dementia know the fear of discovering their loved one has wandered off outside. 
- In similar fashion, HAL 3B00 could be placed at the base of any doorway and serve as a silent tripwire (1000mm/∞ drops to something like 500mm or less)

Using the sensor's *short-distance* mode, the trigger could be the opposite:

- Strategically placed within 1m of your **most prized possession**, HAL 3B00 is a watchdog that could alert you if your stupid younger brother takes it.... or moves it even one millimeter.

## Parts List

- Raspberry Pi 3B+
    - (This project would work fine on the 3A+, too, but a different case would be needed)
    - (Any 4B+ would also work, but this project won't take advantage of its higher specs)
- Micro SD card with standard Raspberry Pi OS
- Appropriate micro-USB Power Supply for the Pi
- SparkFun [Qwiic pHAT 2.0 for Raspberry Pi](https://www.sparkfun.com/products/15945) - *approx. $7*
- SparkFun [Distance Sensor Breakout - 4 Meters (VL53L1X)](https://www.sparkfun.com/products/14722) - *approx. $24*
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

### Temporary Parts
- To setup the OS and program in Python the first time:
    - a USB keyboard and mouse
    - monitor and HDMI cable
- To modify the Smraza case:
    - Xacto knife with 1 blade cartridge that you are willing to ruin
    - Small phillips screwdriver

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
