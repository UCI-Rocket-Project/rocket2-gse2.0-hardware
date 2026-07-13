# GSE Docs
Last updated: 7/12/26

By: Robert Woo

## System Overview

For MOCH4, the GSE (Ground Support Electronics) is a 4-layer PCBA that controls the propulsion feedsystem on the ground. 

It is responsible for data collection from potentiometers, pressure transducers (PTs), and k-type thermocouplers (TC), and controls valve actuation through electric solenoids. 

Control is done entirely throught STM32F105RCT6 microntroller on the board and data collection collected either through the STM32 through the ethernet port on the back of the board or an NiDAQ (Currently a USB-6421) to collect this data from a PC.

![GSE PCB](assets/gse-board.png)

## Inputs and Outputs
| Parameter             | Value                  | Connector                    | Usage         |
|-----------------------|------------------------|------------------------------|---------------|
| Power ports           | 24V DC, 2 redundant ports | 5.5mm barrel jack         | Power board   |
| Ethernet              | 1 port                    | RJ45 uart-to-eth          | Send/Recieve data     
| USB                   | 1 port                    | USB-C                     | Backup/debug data port, not in use|
| NiDAQ port            | 1 port, 16 pins           | 2x9 MOLEX                 | Output sensor voltages to NiDAQ |
| Solenoid ports        | 24V 12 channels/ports     | 15 pin, 3 row DSUB / VGA  | On/Off solenoid actuation |
| Ni PT ports           | 20V 14/15 ports           | 15 pin, 3 row DSUB / VGA  | Finds pressure readings to be read by the NIDAQ |
| STM32 PT ports        | 20V 4 ports               | 15 pin, 3 row DSUB / VGA  | Finds pressure readings to be read by an ADC, data transmitted over eth |
| Ni Loadcell ports     | 10V 2 ports               | 15 pin, 3 row DSUB / VGA  | Finds force readings from a 4 pin load to be read by the NiDAQ |
| STM32 Loadcell ports  | 10V 4 ports               | 15 pin, 3 row DSUB / VGA  | Finds force readings from a 4 pin load cell to be read by an ADC data transmitted over eth |
| TC ports              | 3 ports                   | 2 pin k-type TC plug      | Reads temperature from a k-type TC, data sent over eth | 
| Ematch circuit        | 2 ports, 1 arming port    | Screw terminals           | Sets off an e-match to start engine combustion |

## Circuits
- [Power](power.md)
- [Sensing](sensing.md) 
- [Compute](compute.md) TBD later
- [Communications](communications.md) TBD later
- [Actuation](actuation.md) 