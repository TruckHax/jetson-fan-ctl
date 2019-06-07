# jetson-fan-ctl
Automagic fan control for the jetson nano

## Requirements:

### Hardware
I recommend you use the barrel jack with a 4A power supply  
Additionally, you will need a 5V PWM fan for this to make any sense.  
I used the **Noctua nf-a4x20 5V PWM** fan.

### Software
I will assume you use the standard image on your jetson nano.

If you have python 3, you should be good to go.  
you can check this using <code>python3 --version</code>  
(3.5 or higher should be fine.)  
otherwise, you can install it with  

    sudo apt install python3-dev


## How to install:
add the following lines to /etc/rc.local:

    jetson_clocks
    python3 /path/to/your/jetson-fan-ctl/fanctl.py &

replace <code>/path/to/your/jetson-fan-ctl/fanctl.py</code> 
by the path to fanctl.py in your system.  
The script will automatically run at boot time.

It's a set-it-and-forget-it type thing, unless you want to mess with the fan speeds.

## How to customize:
if you look into fanctl.py, you will find the following lines:

    FAN_OFF_TEMP=20
    FAN_MAX_TEMP=50
    UPDATE_INTERVAL=2

Here you can set your desired temperatures (in °C).  
<code>FAN_OFF_TEMP</code> is the temperature below which the fan is turned off.  
<code>FAN_MAX_TEMP</code> is the temperature above which the fan is at 100% speed.  
The script interpolates linearly between these two points.

Additionally, you can set the interval (in seconds) in which the script updates the fan speed.  
Two seconds worked fine for me here.

You can use either integers (like 20) or floating point numbers (like 20.125) in each of these fields.  
The temperature precision of the thermal sensors is 0.5°C, so don't expect this to be too precise.

Any changes in the script will be will be applied after the next reboot.
