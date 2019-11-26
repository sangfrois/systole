
.. figure::  https://github.com/LegrandNico/systole/blob/master/Images/logo.png
   :align:   center

The **Systole** Python package provide simple tools to record and analyze signal for psychophysiology.
This module is developed inside the ECG group (https://the-ecg.org/). All the scripts are provided with no warranty of any kind.

Installation
============

Download the zip file, extract the folder and run from the terminal:

.. code-block:: shell

  python setup.py install

Recording
=========

Oximeter
--------

Recording signal with the **Nonin 3012LP Xpod USB pulse oximeter** (https://www.nonin.com/products/xpod/) together with the **Nonin 8000SM 'soft-clip' fingertip sensors** (https://www.nonin.com/products/8000s/).

Quick start
###########

Record and plot data with less than 6 lines of code.

.. code-block:: python

  import serial
  from ecgrecording import Oximeter
  ser = serial.Serial('COM4')  # Add your USB port here

  # Open serial port, initialize and plot recording for Oximeter
  oxi = Oximeter(serial=ser, sfreq=75).setup().read(duration=10)

  # Plot data
  oxi.plot()

.. figure::  https://github.com/LegrandNico/systole/blob/master/Images/recording.png
   :align:   center

Recording
#########

2 methods are available to record PPG signal:

* The `read()` method will continuously record for certain amount of
time (specified by the `duration` parameter, in seconds). This is the
easiest and most robust method, but it is not possible to run
instructions in the meantime (serial mode).

.. code-block:: python

  # Code 1 {}
  oximeter.read(duration=10)
  # Code 2 {}

* The `readInWaiting()` method will read all the availlable bytes (up
to 10 seconds of recording). When inserted into a while loop, it allows
to record PPG signal in parallel with other commands.

.. code-block:: python

  import time
  tstart = time.time()
  while time.time() - tstart < 10:
      oximeter.readInWaiting()
      # Insert code here {...}

Online detection
################

Set an online peak detection algorithm in less than 10 lines of code.

.. code-block:: python

  import serial
  import time
  from systole.recording import Oximeter

  # Open serial port
  ser = serial.Serial('COM4')  # Change this value according to your setup

  # Create an Oxymeter instance and initialize recording
  oxi = Oximeter(serial=ser, sfreq=75, add_channels=4).setup()

  # Online peak detection for 10 seconds
  tstart = time.time()
  while time.time() - tstart < 10:
      while oxi.serial.inWaiting() >= 5:
          paquet = list(oxi.serial.read(5))
          oxi.add_paquet(paquet[2])  # Add new data point
          if oxi.peaks[-1] == 1:
            print('Heartbeat detected')

See also a complete tutorial here: <https://github.com/LegrandNico/systole/tree/master/notebooks/HeartBeatEvokedTone.rst>

Peaks detection
###############

Heart rate variability
######################

Interactive visualization
#########################