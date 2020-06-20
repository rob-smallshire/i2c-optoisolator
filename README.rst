i2c-optoisolator
================

An optoisolator for the I²C serial bus which provides electrical isolation between I²C devices
running at up to 400 kHz. Can also be used as a level shifter and repeater. It should also allow the external bus segment to be powered down completely.

The design uses a P82B96PN dual bidirection bus buffer, and four 6N139 darlington opto-isolators to isolate an internal I2C bus from an external I2C bus.

Power
-----

The bidirectional opto-isolator has an internal side, and an external side. The internal side must be powered (VCC-INT to GND-INT), and have a supply suitable for the P82B96PN of 2 V to 15 V. In this design the supply voltage should be the same as the I2C hi logic level of the internal side.  The external side may be powered (VCC-EXT to GND-EXT), and should be powered if the external bus is in use.


Isolation
---------

*** WARNING ***

While the 6N139 opto-isolators support isolation voltages of over 2000 V, these board were designed with low isolation voltages in mind (tens of volts) and have in no way been tested for high potential differences between this internal and external sides. Excerise caution, and take high voltages seriously.

*** END WARNING ***


Component selection
-------------------

X1 and X2
~~~~~~~~~

These are optional 6-way Micro-maTch connectors. See TE Connectivity part number: 7-215464-6 (e.g. Farnell 3398627).

J1 and J2
~~~~~~~~~

These are for standard 0.1" pin headers (or sockets). Note the pin ordering is that originally recommended by Philips/NXP which widely separate clock and data, and is not the same as pinouts which have subsequently become popular in the hobby maker community.

R1 and R4
~~~~~~~~~

Resistors R2 and R4 on the internal side control the current through the internal side LEDs. Select R2 and R4 to give forward current (If) through the LEDs of 0.5 mA assuming a forward LED voltage (Vf) of 1.4 V. For a 5 V supply, an 8k2 resistor is appropriate, though check the datasheet for the brand of isolator you use, as there is some variation in If and Vf values::

  R2 = R4 = (VCC-INT - VLED) / ILED

where,

VCC-INT
  internal side supply voltage

VLED
  Vf - LED forward voltage (1.3 V)

ILED
  If - LED current (0.5 mA)


For example::

  R2 = R4 = (5 V - 1.3 V) / 0.5 mA = 7400R ==> 8k2 resistor

R1 and R3
~~~~~~~~~

Resistors R1 and R3 are pull-ups for the signals arriving from the internal side of the opto-isolators into the buffer. They also serve as current limiting resistors. They should be calculated according to::


  R1 = R3 = (VCC-INT - VOLX) / IL
       
where

VCC-INT
  internal supply voltage

VOLX
  saturation voltage of 6N139 output transistor at current IL - I2 (~ 0.5 V)

IL
  load current through R1 or R3 (say 1 mA)

I2
  input current to P82B96 buffer (0.1 uA)


For example,::

  R1 = R3 = (5 V - 0.5 V) / 1 mA = 4500R ==> 4k7 resistor

R5 and R6
~~~~~~~~~

Resistors R5 and R6 on the external side control the current through the external side LEDs. Select R5 and R6 to give forward current (If) through the LEDs of 0.5 mA assuming a forward LED voltage (Vf) of 1.4 V. For a 5 V supply, an 8k2 resistor is appropriate, though check the datasheet for the brand of isolator you use, as there is some variation in If and Vf values::


  R5 = R6 = (VCC-EXT - VLED) / ILED

where

VCC-EXT
  external side supply voltage

VLED
  Vf - LED forward voltage (1.3 V)

ILED
  If - LED current (0.5 mA)


For example::

  R2 = R4 = (5 V - 1.3 V) / 0.5 mA = 7400R ==> 8k2 resistor


R7 and R8
~~~~~~~~~

Resistors R7 and R8 are optional pull-up resistors for the SDA and SCL lines on the internal side. DO NOT PLACE if the internal side bus already has I2C pull-ups. Values depend on external factors such as bus speed and line capacitance, and battery consumption. Good starting values are 10K for 100 kHz operation, and 2K7 for 400 kHz operation at 5 V.


R9 and R10
~~~~~~~~~~

Resistors R9 and R10 are optional pull-up resistors for the SDA and SCL lines on the external side. DO NOT PLACE if the external side bus already has I2C pull-ups. Values depend on external factors such as bus speed and line capacitance, and battery consumption. Good starting values are 10K for 100 kHz operation, and 2K7 for 400 kHz operation at 5 V. 



