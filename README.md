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

#### Comparator
![Comparator](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/69a6153d9f7088c3379c66fb6a9cb519777054a9/5.PNG)

#### T&H Circuit scematic
![T&H Circuit scematic]()




