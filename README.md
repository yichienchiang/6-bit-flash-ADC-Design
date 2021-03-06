# 6-bit flash ADC Design 

The objective of this project is to design a 100Ms/s 6-bit analog-to-digital converter. The report presents a single-ended flash ADC with 64 reference voltage inputs and 6 outputs from the gray encoder. The track-and-hold circuit is used to track and hold the values of Vin, and the low-power supply dynamic comparator is used to compare the inputs from the track-and-hold circuit and reference voltages. The comparator transfers the compared results into the 2 to 1 completion detectors. Finally, the outputs from the completion detectors are provided to the gray encoder, and 6-outputs result. 


#### ADC Block diagram 
![ADC Block diagram ](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/32b03eb2f485511b1db0ba00f6e660dd54a30343/1.PNG)

#### ADC Schematic
![ADC Schematic](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/46cd0fb09a30c61104170c30d5a3b044e2a330f2/12.png)

## Track and Hold Circuit

Since the threshold voltages of the NMOS and PMOS transistors were 0.7 V and the transmission gates are not reliable, utilizing the track and hold circuit to track and hold the input sine wave signal, Vin, is necessary. The switch of this circuit is a single NMOS transistor. When the clock is high, meaning the circuit is in “track” mode, it would track the input signal.  When the clock is low, in the “hold” mode, and the gate is grounded, the circuit would hold the signal. The comparator uses the hold signal and the reference voltage to do the comparison.

However, the voltage drop at hold mode is unavoidable. The drop is due to the clock-feedthrough from the gate. Because the drop voltage may impact the comparison and then ruin the result, decreasing the voltage drop is necessary. To reduce the effect of the voltage drop, we added a capacitor, C=25fF, at the output (Vout) of the track-and-hold circuit.

#### T&H Circuit scematic
![T&H Circuit scematic](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/68381059f8f6d4830bafffd4086ca99a2b2c68a4/3.PNG)

#### T&H Circuit Simulation
![T&H Circuit Simulation](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/68381059f8f6d4830bafffd4086ca99a2b2c68a4/4.PNG)


## Comparator and comparator completion detection circuit

### Comparator
This comparator design reduces power consumption with a cross-coupled mechanism around the input differential pair and doesn’t add any additional capacitors or complex logic. The cross-coupled mechanism prevents intN and intP from discharging to ground, for small differential inputs. This technique includes a modified preamplifier with a regenerative latch and cross-coupled devices. When the clock is low the circuit is in the reset phase and the regenerative latch is reset, providing no current path to ground. When the clock is high the circuit is in amplification with a discharge path to ground. 

The effect of the amplification stage is that node intN discharges at a reduced rate. As intP and intN decrease in voltage, internal nodes N1and P1 increase. The comparator’s timing behavior ensures that the intP and intN stop discharging at levels Vd1 and Vd2, below Vdd. This proposed scheme saves substantial area at the cost of some added noise. The component gets the input signals from the track-and-hold circuit and resistor ladder. After receiving the signals, the comparator will do the comparison.

#### Comparator Schematic
![Comparator](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/69a6153d9f7088c3379c66fb6a9cb519777054a9/5.PNG)

### Completion detector

The completion detector circuit is for ensuring valid output for unresolved input. When the comparator is in the metastable state, the circuit holds the Vout and Vout_bar high in the original design. After the input voltages separate by more than one threshold voltage, one of the outputs is pulled low by the input. In contrast to a simple gain stage, the circuit guarantees at least one valid output. The circuit operates over a wide range of common-mode input voltages and does not require a clock signal. To connect to the 64-input gray encoder, we chose to use only one output, Vout, to prevent the metastable state and keep the same function of the circuit.

#### Completion detector Schematic
![CDS](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/69a6153d9f7088c3379c66fb6a9cb519777054a9/6.PNG)

#### Simulation results of comparator and completion detection circuit
![Simulation results of comparator and completion detection circuit](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/69a6153d9f7088c3379c66fb6a9cb519777054a9/7.PNG)

### Gray Encoder
Utilizing gray code instead of the traditional binary, has some advantages for the design. The most important is that the transition between two consecutive values for gray code would change only a single bit at a time. This makes it easier to avoid metastability, because we never have more than one bit changing at a time. 

#### Gray Encoder Schematic
![GES](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/0077937897ced309b3933acc96c36870afa538ff/8.PNG)

#### Gray Encoder Schematic detail 
![GESD](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/0077937897ced309b3933acc96c36870afa538ff/9.PNG)

#### Gray Encoder Simulation
![GESim](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/0077937897ced309b3933acc96c36870afa538ff/10.PNG)

#### Gray Encoder Table
![GET](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/05abb1c490a0ee53c33b7780ede29dda0a686ec9/11.PNG)

Since we set the simulation frequency 50 MHz, there would be two clock cycles in a sine wave input period. Therefore, the hold mode of T/H circuit will catch the same two results in a sine period. In other words, if we set the frequency to a non-whole number (For example, 48.4375MHz), the hold mode will catch different values. Basically, at 50 MHz, due to the hold mode catching the same two results every time, the final output of the encoder will have two results in a period.

#### Simulation results of ADC with track-and-hold circuit with Vin = 50 MHz
![Simulation results of ADC with track-and-hold circuit with Vin = 50 MHz](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/05abb1c490a0ee53c33b7780ede29dda0a686ec9/13.PNG)

#### Simulation result when Vin = 48.4375MHz
![Simulation result when Vin = 48.4375MHz](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/05abb1c490a0ee53c33b7780ede29dda0a686ec9/14.PNG)

### FFT Simulation

#### Fs = 40.625MHz, ENOB=4.6598 bits, SNDR= 29.81dB
![F1](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/0d9eba61e20c7d268713dadc0f9ca207efeb9107/16.PNG)

#### Fs = 45.3MHz, ENOB=5.9128 bits, SNDR= 37.35 dB
![F2](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/0d9eba61e20c7d268713dadc0f9ca207efeb9107/16.PNG)

