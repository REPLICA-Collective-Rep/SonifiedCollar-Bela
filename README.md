# SonifiedCollar-Bela
by Mika Satomi for Dancing at the Edge of the World

## Software:
The updated versions are posted here (please check the latest version)>> https://github.com/i-n-g-o/edge_of_the_world-performance
PD control panel:
Preparation
File name: osc-controller_panel.pd, p_panels.pd, all-panels.pd, zmq_sub.pd
You can download from here >> https://github.com/i-n-g-o/edge_of_the_world-performance

Running on PD0.51-1 (vanilla PD)
Necessary externals: cyclone, zmq (https://github.com/sansculotte/pd-zmq you will need to compile it yourself)

Externals:
Add Cyclone: go to find externals, type cyclone
   
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%201.png)
   
Choose the version you want to install. I have 5.3 on mine. You can get a newer one if you like and it should work the same. It installs it in your externals folder

Add zmq:
For Mac, I am including the one we compiled.
For Win or Linux, please download the source code and compile it >> https://github.com/sansculotte/pd-zmq 
Place the zmq externals inside your externals folder.

You need to also install the zmq library.
You can use brew on terminal to install.
Intro to using homebrew >> https://www.howtogeek.com/211541/homebrew-for-os-x-easily-installs-desktop-apps-and-terminal-utilities/
Open terminal, type “brew install zmq” (see the second line)
Then it should install. (I already had it installed, so I get an error stating I already have it.)

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%202.png)

You will need to add the path to your externals folder from preference. Check where your created your externals folder.

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%203.png)

Check if the zmq and gate object is recognized. It is placed on the right corner. If you see it with a dotted line, the externals are not recognized and the patch will not function properly.
 
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%204.png)

Go to your OS and connect your computer to the same network as BELA devices
If you are using router from Meredith, it is network name: “things”  password: “shape2020”
Make sure that proxy server is running (it is inside the raspberry pi, make sure to turn on the power)
 
## Using the controller
Open ontroller_panel.pd (place all three patches in the same folder)

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%205.png)
 
### Individual control panel:

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%206.png)

<p>SENSOR: shows the sensor reading of this device. If calibrated, it shows after applying calibration.</p>

<p>MAPPED: it shows what values are controlling the synthesizer. In Mode 2-4, it shows the result from Machine Learning.</p>

<p>CONTROL: only used in mode 2-4. The target value sent for training ML. When you press the presets, it shows which values are set as the target. You can also move the slider and adjust the target before training.</p>

<p>mode: sound synthesis mode changer. 0: Directly mapping sensor data to volume of 8 oscillator, 1: acceleration of each sensor data controlling volume of 8 oscillator, 2: Voder, synthesized voice, 3: Phoneme, triggers sound files in cue, 4: Granular synthesis</p>

<p>random: toggles between sensor data as input and random walk data as input. Mostly used for wifi signal tests.</p>

<p>calib: when on, it adjusts the max/min range and uses it to scale the sensor data. Turn on, ask the performer to move/give the sensor min/max range of movements, and turn off. When turned off, it writes the value to a file and it is always loaded when the BELA is turned on next time.</p>

<p>Set base: used in mode 0. Sets the base position (i.e. standing) and takes the value difference from this point. This is so that when using mode 0, it sets the quiet point as default.</p>

<p>Volume: controls volume</p>

Yellow bang next to the volume shows that data is received from the device. Indicator for device connection. (this also slows down the GUI, so it maybe necessary to disconnect if it becomes a problem) >> this is out now as it slows down the whole GUI

In mode 2,3,4, it uses Machine Learning to map sensor data to control data. When you switch to the mode the first time after booting Bela, it trains to the last data you have written. This is performed 1sec after you switch the mode, and takes a few seconds. You may hear strange glitch sound as the ML takes very much calculation power in training process.

Training process
Instead of using last training data, you can train it with new poses.
Here is how you can train it step by step.

-	Go to the mode you want to train. Let’s say you choose mode 2, voder.
-	Toggle train_mode (pink toggle button) on (shows X inside). 
-	Clear the last data by clicking “ml_clear”
-	Click preset from your mode to show the target value to the ML

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%207.png)

 	The first in the list is always the quiet value. When you click through, CONTROL slider should be changing and you can hear the target sound from individual speakers. You can also move the CONTROL slider to adjust the target value
If you use presets from other mode, (i.e. granular preset in voder) it works, but will sound strange.
-	Ask the performer to be on the pose you want to train
-	Toggle “add data” (turquoise blue button) on (x inside) for 1-2 second and turn off. When clicked again, it becomes off (no x inside). Do not keep it on too long. When the training data is too big, it fails to train. 
-	Iterate this process to all the preset you want to train.
-	When you are done with the iteration, click “ml_write” to overwrite the existing training data file (you can skip writing, then the training data will not stay for the next time you turn on Bela).  Then click “ml_train”. This triggers the training process. It takes a few seconds to finish this process. You may hear glitchy sound from the speaker as it disturbs synthesizing.
-	Lastly toggle train_mode (pink toggle button) off (shows no X inside). 

If the training is not working as well as you wanted, for example, the quiet pose is not very quiet, then you can go back to the training mode, do not clear the ml (no “ml_clear”), click desired preset (i.e. quiet preset), ask the quiet pose to be made and add data, ml_train. This will add the pose in existing training mode.

When you are out of train_mode, you will start to see SENSOR data controlling MAPPED data. 

### ALL control panel:

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%208.png)

 	When you use CONTROL ALL panel, you can control all the connected devices at once. The functionality of each buttons are the same as the individual ones.

When you click get_ip, you will get all the connected device in in PD window (command-R)

Random triggers random walk mode (randomly generated sensor data). This is used for development process.


#### When you want to change the preset value:
Preset values are saved in each preset subpatches. Ctrl-click the preset panel and choose “open”. This will open the subpatch. Command-E to enter edit mode, and you can change/edit the preset that is saved in message boxes.

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%209.png)
 
When you want to save the preset that you manually changed on the CONTROL slider, or mapped values came out from performers, you can click “grab_preset” in individual control panels. This will result in printing out the list to this message box on the right bottom corner. You can copy this box, or partly copy the number and paste in the preset subpatch.
 
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2010.png)

## Connecting directly to Bela:
You can connect Bela directly from the browser or from ssh in the terminal.
Here I note how to access using a browser.
Make sure that your computer is connected to the same network as Bela and proxy.

Get ip address of bela using “get_ip” from PD controller. (you can alternatively send “control all system ip” to tcp://192.168.0.10:5556 socket:publish)

Type in the ip address you want to connect. On the browser's ip address box. You will get the GUI of Bela

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2011.png) 

Alternatively, you can try the address “bela.local” , but if you have multiple Bela on network, it will not work.
If you connect Bela on USB cable, you can access Bela over the USB. in this case the address is 192.168.7.2(mac) or 192.168.6.2 (windows, but sometimes on mac too)

Currently Bela is set so that it will automatically connect to WIFI (things) and run the patch (_main.pd in “simple_kling” folder) for the performance every time it is booted. 

If you need to stop or restart the patch (for example, it can fail the ML training and it can stop), you can click the “build and run” or “stop” button on the left bottom corner)

If you need to update files (pd patches or sound files), select the correct folder from “your projects”, drop-in the files you want to update, stop the patch, and “build and run” the patch. 


## Maintenance:
Battery: Plug out the USB-A cable from the battery to turn off when you do not use it.
You will need to make sure that the battery is charged. There are two batteries. One on the back, a black power bank, and the one included in the portable speakers. Both take microUSB cable to charge it. 
On the power bank battery, the power is charged through the micro USB plug, and it comes out from USB A plug. Do not mix up!

You can use normal USB power adapters to charge these devices. Speaker lasts 5h when fully charged. Power Bank also can provide a few hours of power to Bela (not tested exactly how long..)

Speaker inlet for microUSB to charge.
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2012.png)  
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2013.png)
Power Bank. USB micro (left most) to charge. USB-A (left most, big one) is to get the power out to Bela.
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2014.png)
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2015.png)
To charge, you can use android phone charging cable/plugs like this one
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2016.png)
![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2017.png)

## Sensor Orders:
Please place the sensors on bodies according to this order. It will be easier if you wear the collar first, drop the cable+sensor through your shirt/pants and fix the sensor using skin glue and kinesiology tape. 

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2018.png)

Bend sensors are connected already to the cables. Each sensor connects with one blue and one dark blue cable. You can switch blue/dark blue order on the sensor. It will not affect the readings.

Please use eyelash/skin glue and kinesiology tape (you can buy them in DM) to place the sensors on your body. 
On the collar, your name inicial is embroidered (KI for Kirsty, KA for Kate, Dm for Dimitri). Please use the one with your name as it corresponds to the controller panel. There are slight differences in cable lengths according to your height. 

![image](https://github.com/REPLICA-Collective-Rep/SonifiedCollar-Bela/blob/main/Pictures/Picture%2019.png)

 

