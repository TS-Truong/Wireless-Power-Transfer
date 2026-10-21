# Wireless-Power-Transfer
Design and LTspice simulation of a 6.78 MHz and 13.56 MHz GaN-based Class E amplifier for wireless power transfer.

## Overview
This project was completed by a four member, Texas Instruments-sponsored, senior design team at Southern Methodist University. The project's objective was to develop a wireless power transfer device capable of powering an LED through a 6 cm drywall. Through multiple iterations, the final prototype operated at 6.78 MHz and delivered 0.667W to the LED.

## My Contribution
My primary responsibility in the team was the design and implementation of the Class E amplifier. I developed and refined amplifier designs using LTspice, adjusting circuit parameters to achieve zero-voltage switching (ZVS). I also sourced components and assembled the amplifier on the respective development board. This repository primarily documents my amplifier design process, LTspice simulations, and associated waveforms.

## Design Methodology
### Theoretical Design
The amplifier design was based on the Class E topology described in [N. O. Sokal's Class-E RF Power Amplifiers](https://link.springer.com/chapter/10.1007/0-306-47950-8_14). The circuit uses a GaN transistor as a high-frequency switch, RF choke inductor, a shunt capacitor, and resonant circuit. Initial component values were calculated using Sokal's design equations, which relate the components value to operating frequency, load resistance, and loaded quality factor.

### LTspice Simulation and Tuning
The amplifier was modeled in LTspice using EPC GaN transistor models. A resistive load was used to approximate the transmitting antenna, following the simplified load model presented in Sokal's paper. This approximation does not capture the antenna's reactive impedance.

Starting from the calculated component values, the circuit was iteratively tuned by adjusting the shunt capacitance and resonant network components. Following Sokal's tuning methodology, these adjustments were used to shape the drain source voltage waveform so that it approached zero with approximately zero slope at transistor turns on, producing switching behavior consistent with ZVS.

## Design Iterations
### Proof of concept (10MHz)
![Schematic of the proof of concept](Figures/Proof%20of%20concept%20-%2010MHz/Class%20E%20amplifier%20schematic.png)
The initial simulation was developed at 10 MHz to verify and test out the findings of the Sokal's paper before selecting a specific operating frequency or GaN transistor. The frequency, voltage, and load resistance was chosen for calculation convenience.

A simplified voltage controlled switch from LTspice library was used instead of a specific GaN transistor. Component values were calculated using Sokal's design equations and implemented directly without further tuning. The resulting drain voltage waveform reached approximately zero when the transistor turned on, demonstrating ZVS.

This proof of concept established a starting point for subsequent designs incorporating device-specific transistor characteristics.

### Initial GaN Design (13.56MHz)
![Schematic of the initial design](Figures/Initial%20design%20-%2013.56MHz/Class%20E%20amplifier%20schematic.png)
In the next iteration, the targeted frequency was set to 13.56MHz (a frequency in the ISM band) and GaN transistor EPC2034C was selected. Both were chosen due to their expected performance and efficiency. Unlike the proof of concept, this simulation used a specific transistor model, requiring adjustments to the circuit components to obtain the desired ZVS.

Although the design demonstrated ZVS in LTspice, hardware implementation presented practical challenges. Many of the components were too small for reliable manual soldering, and a suitable clock capable of delivering a 13.56 MHz switching signal to the gate could not be identified.

These implementation constraints resulted in a revision of the operating frequency and transistor selection.

### Final Design
![Schematic of the final design](Figures/Final%20design%20-%206.78MHz/Class%20E%20amplifier%20schematic.png)
For the final iteration, the targeted frequency was reduced to 6.78 MHz (another frequency in the ISM band) to make the prototyping more feasible. In addition, GaN transistor EPC2034C was replaced with the EPC2207 because its development board offered a circuit topology better suited to implementing the simulated amplifier. Lastly, the transmitting antenna was also revised by increasing the number of turns. As a result, the resistive load was now 5Ω, and the DC supply voltage was increased from 10V to 20V to provide greater output for the LED. Similar to the previous iterations, the circuit component values were recalculated and subsequently tuned following Sokal's methodology for ZVS.

The final system successfully delivered power to an LED through 6 cm of drywall. However, ZVS was verified only through simulation, not through measurements of the physical transistor.

## Simulation Result
### Transistor Switching Behavior
The simulation of the 6.78 MHz final design produced the characteristic of a class E amplifier. When the transistor is off, the drain voltage rises and falls as the resonant network shapes the waveform. The voltage approaches zero when the  transistor turns on and remains near zero during the on-state, demonstrating switching behavior consistent with ZVS.
![Amplifier VG, VDS, and IDS](Figures/Final%20design%20-%206.78MHz/Class%20E%20amplifier%20switching.png)

### Amplifier Output 
The final design produced an approximately sinusoidal voltage and current waveform across the 5 Ω resistive load at 6.78 MHz. This demonstrates the resonant network's ability to produce a high frequency AC output intended to drive the transmitting antenna.
![Amplifier output](Figures/Final%20design%20-%206.78MHz/Class%20E%20amplifier%20output.png)

## Running the Simulations
### Requirements
* [LTSpice](https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html)
* [EPC GaN LTspice library](https://epc-co.com/epc/design-support/device-models) (require for 13.56MHz and 6.78MHz designs)

### Instructions
* Clone or download this repository.
* Download the EPC GaN LTspice model library from the link above and extract EPCGaNLibrary.lib.
* Place EPCGaNLibrary.lib in the same directory as the .asc and .asy files for the corresponding GaN amplifier simulation.
* Open the desired .asc schematic in LTspice.
* Click Run to execute the transient simulation.

### Viewing the Waveforms
The default .plt file automatically displays both transistor switching and amplifier output waveforms when the simulation is run. Separate .plt files are also provided for the individual switching and output plots shown in this README. These can be loaded through Plot Settings → Open Plot Settings File in LTspice.