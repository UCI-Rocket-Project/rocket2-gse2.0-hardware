# Overview
Last updated: 7/12/26

By: Robert Woo

The GSE supports 3 actuator types: Solenoid Valves, E-matches, and the Alarm.

The GSE includes:

- 12 Solenoid Valve output channel
- 2 E-match output channels
- 1 Alarm output channels


# Solenoid Valves

![Solenoid Valve Circuit](assets/solenoid-valve-circuit.png)

The GSE includes 12 solenoid valve out channels.

Solenoids are basically as inductors, which is why D10 is there. It's called a flyback diode, and its purpose is the allow the solenoids
to dissipate its stored magnetic field from an on-to-off state. D10's model has insanely high rating to deal with transients when current
on and off through an inductive element.

R31, R30, and Q2 are used to turn on and off the solenoid through the MCU's GPIO pin. R30 is for nominal off-state and R31 is to deal with overshoots in MOSFET gate voltages.

R32 should be a high wattage, 1 ohm resistor that is used primarily to read if there is current passing through the solenoid electrically.
Our solenoids draw about 500mA under 24V, so V=IR shows us that the voltage above R32 should be about 0.5V

The remaining components are double protection send this 0.5V back to an ADC pin on GSE's MCU. D11 is a zener diode which in combination 
with R33 will clamp unsafe voltage to the op-amp (U9) and pass thr current to ground. U9 just passed whatever voltage is at the + pin on 
op-amp, but acts as a buffer between the potential 24V that this circuit can see and potentially send to the MCU. You don't want to blow 
up the MCU by sending it 24V.

# E-matches

![E-match Circuit](assets/ematch-circuit.png)

The GSE includes 2 e-match output channels.

The e-match driver circuit uses a two-stage MOSFET switching configuration to safely control current delivery to the e-match load and read status of the circuit.

In the image, the node PWR will see either 0 or 24V depending on the if the arming is key is switched on or not to pass through the 
voltage

The CONT pin is a GPI that reads a logical high or low depending if there is voltage at the gate of the mosfet (Q12). If there is voltage
there, that means:

* the key is armed
* an ematch is connected between the screw terminals on GSE

R75 and R76 are used to step down the voltage to the gate of MOSFET to keep the voltages of the MOSFET in its operating range

R73, R72, and Q11 are used actually start an ematch. if FIRE goes to 3.3V from the MCU, the MOSFET will complete the circuit for the 
ematch to blow it.

**R74 is really important.** The MOSFET (DMG3402L) has a max current between drain and source (Id) of 4.0A. **R74 is there so that the MOSFET never fails from overcurrent. THe failure mode from overcurrent is the MOSFET always completing the circuit** 

# Alarm

![Alarm Circuit](assets/alarm-circuit.png)

The GSE includes only 1 alarm output channel.

In the alarm driver circuit, a MCU GPIO control signal (ALARM) drives the gate of an N-channel MOSFET through a series resistor (R14). In parallel, a pull-down resistor (R13) tied to GND ensures a defined OFF state and prevents the build-up of residual charge.

Simple low-side mosfet switch
